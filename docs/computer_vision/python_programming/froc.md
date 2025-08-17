# Precision Recall Curve



# FROC

```python
FROC_data = np.zeros((4, len(roidb)), dtype=np.object)      
FP_summary = np.zeros((2, len(roidb)), dtype=np.object)
detection_summary = np.zeros((2, len(roidb)), dtype=np.object)
thresh = 0.1
for i, entry in enumerate(roidb):
    image_name = entry['file_name']
    mask, label = get_segm_mask(entry) 
    bboxs, segms, scores = get_predicts(image_name, bboxs_data, segms_data)
    FROC_data[0][i] = image_name
    FP_summary[0][i] = image_name
    FROC_data[0][i] = image_name
    FROC_data[1][i], FROC_data[2][i], FROC_data[3][i], detection_summary[1][i], FP_summary[1][i] = compute_FP_TP_Probs(mask, segms, scores, thresh)
total_FPs, total_sensitivity, all_probs = computeFROC(FROC_data)
```

```python
def if_mask_overlap(mask, predict_mask, thresh=0.5):
    predict_size = np.sum(predict_mask > 0 * 1)
    for i in range(np.amax(mask)):
        label_size = np.sum(mask==(i+1) * 1)
        intersect = np.sum((mask==(i+1)) * (predict_mask > 0) * 1)
        if (intersect * 1. / label_size) > thresh or (intersect * 1. / predict_size) > thresh:
            return i+1
    return 0

def get_predicts(image_id, bboxs_data, segms_data):
    predicts = []
    scores = []
    segms = []
    for i, candidate in enumerate(bboxs_data):
        if candidate['image_id'] == image_id:
            scores.append(candidate['score'])
            predicts.append(candidate['bbox'])
            segms.append(segms_data[i]['segmentation'])
    return predicts, segms, scores

def get_label_mask(name, image_dir, label_dir):
    unique_label = []
    label_json = label_dir + name +'.txt'
    label_data = read_json(label_json)
    image = cv2.imread(image_dir + name + '.png')
    mask = np.zeros(image.shape, dtype='uint8')
    i = 0
    for nodule in label_data['nodes']:
        if nodule['type'].lower() == 'mass':
            contours = nodule['rois'][0]['edge']
            if len(contours) > 3:
                i += 1
                if isinstance(contours[0][0], unicode):
                    flat_list = [max(0, float(item)) for sublist in contours for item in sublist]
                else:
                    flat_list = [max(0, item) for sublist in contours for item in sublist]
                points = np.array(flat_list, dtype=np.int32).reshape((-1, 2))
                label_bbox = cv2.boundingRect(points[:, np.newaxis, :])
                if label_bbox not in unique_label:
                    unique_label.append(label_bbox)
                    cv2.drawContours(mask, (points, ), 0, color=i, thickness=-1)
    return mask[:,:,0], unique_label

def compute_FP_TP_Probs(mask, predict_segms, Probs, thresh):
    max_label = np.amax(mask)
    FP_probs = []
    TP_probs = np.zeros((max_label,), dtype=np.float32)
    detection_summary = {}
    FP_summary = {}
    for i in range(1, max_label+1):
        label = 'Label' + str(i)
        detection_summary[label] = []
    FP_counter = 0
    if max_label > 0:
        for i, predict_segm in enumerate(predict_segms):
            predict_mask = mask_util.decode(predict_segm)
            HittedLabel = if_mask_overlap(mask, predict_mask, thresh)
            if HittedLabel == 0:
                FP_probs.append(Probs[i])
                key = 'FP' + str(FP_counter)
                FP_summary[key] = [Probs[i], predict_segm]
                FP_counter += 1
            elif (Probs[i] > TP_probs[HittedLabel - 1]):
                label = 'Label' + str(HittedLabel)
                detection_summary[label] = [Probs[i], predict_segm]
                TP_probs[HittedLabel-1] = Probs[i]
    else:
        for i, predict_segm in enumerate(predict_segms):
            FP_probs.append(Probs[i])
            key = 'FP' + str(FP_counter)
            FP_summary[key] = [Probs[i], predict_segm]
            FP_counter += 1
    return FP_probs, TP_probs, max_label, detection_summary, FP_summary


def computeFROC(FROC_data):
    unlisted_FPs = [item for sublist in FROC_data[1] for item in sublist]
    unlisted_TPs = [item for sublist in FROC_data[2] for item in sublist]

    total_FPs, total_TPs = [], []
    all_probs = sorted(set(unlisted_FPs + unlisted_TPs))
    for Thresh in all_probs[1:]:
        total_FPs.append((np.asarray(unlisted_FPs) >= Thresh).sum())
        total_TPs.append((np.asarray(unlisted_TPs) >= Thresh).sum())
    total_FPs.append(0)
    total_TPs.append(0)
    total_FPs = np.asarray(total_FPs) / float(len(FROC_data[0]))
    total_sensitivity = np.asarray(total_TPs) / float(sum(FROC_data[3]))
    return total_FPs, total_sensitivity

def save_FP_TP(save_name, total_FPs, total_sensitivity):
    save_dir = '/data1/luoling/result/FROC/'
    result = []
    result.append(total_FPs)
    result.append(total_sensitivity)
    save_object(result, save_dir + save_name)
def load_FP_TP(file_name):
    save_dir = '/data1/luoling/result/FROC/'
    from six.moves import cPickle as pickle
    result = pickle.load(open(save_dir + file_name, 'rb'))
    return result[0], result[1]
def plotFROC(tpr, fpr):
    fpr1, tpr1 = load_result('rec')
    fpr2, tpr2 = load_result('vgg16_4')
    fpr3, tpr3 = load_result('vgg16_8')
    fig = plt.figure()
    plt.xlabel('False Positive per Image', fontsize=12)
    plt.xlim((0.1,100))
    plt.ylabel('Recall', fontsize=12)
    fig.suptitle('FROC on in-house dataset', fontsize=12)
    line1, = plt.semilogx(fpr1, tpr1, '-', color='b', label="Proposed")
    line2, = plt.semilogx(fpr2, tpr2, '-', color='r', label="VGG16/4")
    line3, = plt.semilogx(fpr3, tpr3, '-', color='g', label="VGG16/8")
#     plt.semilogx(fpr, tpr, '-', color='k', label="VGG16_8")
#     legend1 = plt.legend(handles=[line1], loc=4)
#     ax = plt.gca().add_artist(legend1)
    plt.legend(handles=[line1, line2, line3], loc=4)
    plt.grid(True, which='both')
    plt.savefig('FROC_domestic.eps', format='eps')
    plt.show()
```