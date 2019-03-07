```python
#coding: utf-8
import pandas as pd
import tqdm
import SimpleITK as sitk
import numpy as np
import cv2
from scipy.misc import toimage, imsave
from glob import glob
import os
import json
from data.dm_preprocess import DMImagePreprocessor as imprep
import sys

json_dir = '/home/HDD1/mammogram/DDSM/json_v3/'

root_dir = '/home/HDD1/mammogram/DDSM/'
out_dir = '/home/HDD1/mammogram/DDSM/patch/all_318'

calc_train_dir = 'Calc-Test_Full_Mammogram_Images/'
calc_train_roi_dir = 'Calc-Test_ROI_and_Cropped_Images/'

bkg_dir=os.path.join(out_dir, 'background')
calc_pos_dir=os.path.join(out_dir, 'calc_mal')
calc_neg_dir=os.path.join(out_dir, 'calc_ben')
mass_pos_dir=os.path.join(out_dir, 'mass_mal')
mass_neg_dir=os.path.join(out_dir, 'mass_ben')

target_height = 1344
patch_size = 318 #224
nb_abn = 10
nb_bkg = 10
nb_hns = 5
pos_cutoff = 0.9
neg_cutoff = 0.2
verbose = False
rng = np.random.RandomState(12345)

problem_case = []

def read_img(fname):
    image_array = []
    if fname.split('.')[-1] == 'dcm':
        image = sitk.ReadImage(fname)
        image_array = sitk.GetArrayFromImage(image)
        image_array = np.squeeze(image_array)
    return image_array
def crop_img(img, bbox):
    x_min,y_min,x_max,y_max = bbox
    return img[x_min:x_max, y_min:y_max]


def add_img_margins(img, margin_size):
    '''Add all zero margins to an image
    '''
    enlarged_img = np.zeros((img.shape[0]+margin_size*2,
                             img.shape[1]+margin_size*2))
    enlarged_img[margin_size:margin_size+img.shape[0],
                 margin_size:margin_size+img.shape[1]] = img
    return enlarged_img

def crop_val(v, minv, maxv):
    v = v if v >= minv else minv
    v = v if v <= maxv else maxv
    return v



def sample_blob_negatives(img, roi_mask, basename, blob_detector, nb_bkg=10, start_sample_nb=0):
    key_pts = blob_detector.detect((img/img.max()*255).astype('uint8'))
    key_pts = rng.permutation(key_pts)
    sampled_bkg = 0
    for kp in key_pts:
        if sampled_bkg >= nb_bkg:
            break
        x, y = int(kp.pt[0]), int(kp.pt[1])
        if not overlap_patch_roi((x, y), patch_size, roi_mask, cutoff=neg_cutoff):
            patch = img[y-patch_size/2:y + patch_size/2,
                        x-patch_size/2:x + patch_size/2]
            # 全黑去掉
            if patch.mean() / patch.max() > 10.0/255.0:
                patch = patch.astype('int32')
                patch_img = toimage(patch, high=patch.max(), low=patch.min(), mode='I')
                filename = basename + "_%02d" % (start_sample_nb + sampled_bkg) + ".png"
                fullname = os.path.join(bkg_dir, filename)
                imsave(fullname, patch_img)
                if verbose:
                    print "sampled a blob patch at (x,y) center=", (x,y)
                sampled_bkg += 1
    return sampled_bkg

def overlap_patch_roi(patch_center, patch_size, roi_mask,
                      add_val=1000, cutoff=.5):
    x1,y1 = (patch_center[0] - patch_size/2,
             patch_center[1] - patch_size/2)
    x2,y2 = (patch_center[0] + patch_size/2,
             patch_center[1] + patch_size/2)
    x1 = crop_val(x1, 0, roi_mask.shape[1])
    y1 = crop_val(y1, 0, roi_mask.shape[0])
    x2 = crop_val(x2, 0, roi_mask.shape[1])
    y2 = crop_val(y2, 0, roi_mask.shape[0])
    roi_area = (roi_mask > 0).sum()
    roi_patch_added = roi_mask.copy()
    roi_patch_added[y1:y2, x1:x2] += add_val
    patch_area = (roi_patch_added>=add_val).sum()
    inter_area = (roi_patch_added>add_val).sum().astype('float32')
    return (inter_area/roi_area > cutoff or inter_area/patch_area > cutoff)

def create_blob_detector(roi_size=(128,128), blob_min_area=3,
                         blob_min_int=.5, blob_max_int=.95, blob_th_step=10):
    params = cv2.SimpleBlobDetector_Params()
    params.filterByArea = True
    params.minArea = blob_min_area
    params.maxArea = roi_size[0]*roi_size[1]
    params.filterByCircularity = False
    params.filterByColor = False
    params.filterByConvexity = False
    params.filterByInertia = False
    # blob detection only works with "uint8" images.
    params.minThreshold = int(blob_min_int*255)
    params.maxThreshold = int(blob_max_int*255)
    params.thresholdStep = blob_th_step
    ver = (cv2.__version__).split('.')
    if int(ver[0]) < 3:
        return cv2.SimpleBlobDetector(params)
    else:
        return cv2.SimpleBlobDetector_create(params)

def sample_patchs(img, roi_mask, basename, pos,
                  nb_bkg=10,
                  start_sample_nb=0, itype='mass', pos_cut=.75, neg_cut=.35):
    print(basename)
    if pos == 1:
        if itype == 'mass':
            roi_out = mass_pos_dir
        else:
            roi_out = calc_pos_dir
    else:
        if itype == 'mass':
            roi_out = mass_neg_dir
        else:
            roi_out = calc_neg_dir

    # 根据图像的矩计算重心
    roi_mask_8u = roi_mask.astype('uint8')
    ver = (cv2.__version__).split('.')
    if int(ver[0]) < 3:
        contours, _ = cv2.findContours(roi_mask_8u.copy(), cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    else:
        _, contours, _ = cv2.findContours(roi_mask_8u.copy(), cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    cont_areas = [cv2.contourArea(cont) for cont in contours]
    if len(cont_areas) < 1:
        problem_case.append(basename)
        return False
    idx = np.argmax(cont_areas)  # find the largest contour.
    rx,ry,rw,rh = cv2.boundingRect(contours[idx])
    if verbose:
        M = cv2.moments(contours[idx])
        try:
            cx = int(M['m10'] / M['m00'])
            cy = int(M['m01'] / M['m00'])
            print "ROI centroid=", (cx, cy); sys.stdout.flush()
        except ZeroDivisionError:
            cx = rx + int(rw / 2)
            cy = ry + int(rh / 2)
            print "ROI centroid=Unknown, use b-box center=", (cx, cy)

    sampled_abn = 0
    nb_try = 0

    while sampled_abn < nb_abn:
        if nb_abn > 1:
            x = rng.randint(rx, rx + rw)
            y = rng.randint(ry, ry + rh)
            nb_try += 1
            if nb_try >= 1000:
                print "Nb of trials reached maximum, decrease overlap cutoff by 0.05"
                pos_cut -= .05
                nb_try = 0
                print pos_cut
                if pos_cut <= .0:
                    raise Exception("overlap cutoff becomes non-positive, "
                                    "check roi mask input.")
        else:
            x = cx
            y = cy
        if nb_abn == 1 or overlap_patch_roi((x, y), patch_size, roi_mask,
                                             cutoff=pos_cut):
            patch = img[y - patch_size / 2:y + patch_size / 2,
                        x - patch_size / 2:x + patch_size / 2]
            patch = patch.astype('int32')
            patch_img = toimage(patch, high=patch.max(), low=patch.min(),
                                mode='I')

            filename = basename + "_%02d" % (sampled_abn) + ".png"
            fullname = os.path.join(roi_out, filename)
            imsave(fullname, patch_img)
            sampled_abn += 1
            nb_try = 0
    # Sample background
    sampled_bkg = start_sample_nb
    while sampled_bkg < start_sample_nb + nb_bkg:
        x = rng.randint(patch_size / 2, img.shape[1] - patch_size / 2)
        y = rng.randint(patch_size / 2, img.shape[0] - patch_size / 2)
        if not overlap_patch_roi((x, y), patch_size, roi_mask, cutoff=neg_cut):
            patch = img[y - patch_size / 2:y + patch_size / 2,
                        x - patch_size / 2:x + patch_size / 2]
            # 全黑去掉
            if patch.mean() / patch.max() > 10.0/255.0:
                patch = patch.astype('int32')
                patch_img = toimage(patch, high=patch.max(), low=patch.min(),
                                    mode='I')
                filename = basename + "_%02d" % (sampled_bkg) + ".png"
                fullname = os.path.join(bkg_dir, filename)
                imsave(fullname, patch_img)
                sampled_bkg += 1
                if verbose:
                    print "Sampled a bkg patch at (x,y) center=", (x,y)
                    sys.stdout.flush()
    return True


def sampling(json_file):
    blob_detector = create_blob_detector(roi_size=(patch_size, patch_size))
    img_id = json_file.split('/')[-1].split('.')[0]
    f = open(json_file, 'r')
    data = json.load(f)
    f.close()
    bbox = np.array(data['other_info']['bbox'])
    image_path = str(data['other_info']['dataPath'])
    bkg_sampled = False
    for nodule in data['nodes']:
        pos = nodule['attr']['malignant']
        points = np.array(nodule['rois'][0]['edge'])
        abn = nodule['name']
        itype = abn.split('_')[0]
        basename = '_'.join([img_id, abn])
        image = read_img(image_path)
        roi_mask = np.zeros(image.shape, np.uint8)
        cv2.fillPoly(roi_mask, (points,), (255, 255, 255))
        roi_mask = crop_img(roi_mask, bbox)
        image = crop_img(image, bbox)

        # Resize到固定高度
        if target_height is not None:
            target_width = int(float(target_height) / image.shape[0] * image.shape[1])
            roi_mask = cv2.resize(roi_mask, dsize=(target_width, target_height),
                                  interpolation=cv2.INTER_CUBIC)
            image = cv2.resize(image, dsize=(target_width, target_height),
                               interpolation=cv2.INTER_CUBIC)

        roi_mask = add_img_margins(roi_mask, patch_size / 2)
        image = add_img_margins(image, patch_size / 2)

        nb_hns_ = nb_hns if not bkg_sampled else 0
        if nb_hns_ > 0:
            hns_sampled = sample_blob_negatives(image, roi_mask, basename, blob_detector, nb_hns_, 0)
        else:
            hns_sampled = 0
        nb_bkg_ = nb_bkg - hns_sampled if not bkg_sampled else 0
        sample_patchs(image, roi_mask, basename, pos, nb_bkg_, hns_sampled, itype, pos_cutoff, neg_cutoff)
        bkg_sampled = True

def main():
    json_files = glob(json_dir + '*.txt')
    i = 0
    for json_file in json_files:
        sampling(json_file)
        print('index: %d' % i)

        i += 1

    print(problem_case)

if __name__=='__main__':
    main()
    # json_file = glob(json_dir + '00047_LEFT_MLO.txt')[0]
    # sampling(json_file)
```