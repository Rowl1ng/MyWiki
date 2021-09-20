# 可视化

随机 
```python
random.shuffle(images)
```

```python
fig = plt.figure(figsize=(10,10))
image = cv2.imread(image_path.replace('origin_sym', 'origin'))
ax1 = fig.add_subplot(111, aspect='equal')
ax1.imshow(image,cmap='gray')
bboxes = result['bboxes']
contours = result['segms']
for i, item in enumerate(bboxes):
    x1, y1, x2, y2 = item[:4]
#             score = item[-1]
#     ax2.plot(contour[:, 1], contour[:, 0], linewidth=2)
    rect = patches.Rectangle(
            (x1, y1),   # (x,y)
            x2-x1,          # width
            y2-y1,          # height
        linewidth=2,edgecolor='g',facecolor='none'
        )
    ax1.add_patch(rect)
    contour = contours[i] # (x, y)
    ax1.plot(contour[:, 1], contour[:, 0], linewidth=2)
# plt.imshow(image)
```

```python
def vis_detect(image_name, thresh=0.0):
    image = cv2.imread(image_dir + image_name + '.png', 0)
    json_file = label_dir + image_name + '.txt'
    image_data = read_json(json_file)
    fig = plt.figure(figsize=(20,40))
    ax1 = fig.add_subplot(121, aspect='equal')
    ax1.imshow(image,cmap='gray')
    gt = 0
    for ix, nodule in enumerate(image_data['nodes']):
        contours = np.array(nodule['rois'][0]['edge'])
        if nodule['type'].lower() == 'mass':
            points = contours[:, np.newaxis, :]
            x1, y1, w, h = cv2.boundingRect(points)
#             print(x1, y1, w, h)
            rect = patches.Rectangle(
                    (x1, y1),   # (x,y)
                    w,          # width
                    h,          # height
                linewidth=2,edgecolor='r',facecolor='none'
                )
            ax1.add_patch(rect)
            gt += 1
    ax2 = fig.add_subplot(122, aspect='equal')#name, shape, image_dir, label_dir
    mask, label = get_label_mask(image_name, image.shape, image_dir, label_dir) 
    predicts, segms, scores = get_my_predicts(thresh, image_name)
    for bbox, predict_segm, score in zip(predicts, segms, scores):
        if if_mask_overlap(mask, predict_segm, 0.5) > 0:
            print  bbox, score
            x1,y1,w,h = bbox
            segm_mask = mask_util.decode(predict_segm)
            if segm_mask.sum() > 0:
                contours = find_contours(segm_mask, 0.5)
    #                 print len(contours)
                contour = contours[0]
                ax2.plot(contour[:, 1], contour[:, 0], linewidth=2)
            rect = patches.Rectangle(
                    (x1, y1),   # (x,y)
                    w,          # width
                    h,          # height
                linewidth=2,edgecolor='g',facecolor='none'
                )
            ax2.add_patch(rect)

    ax2.imshow(image,cmap='gray')
```

```python
def vis_detections(im, dets, CONF_THRESH = 0.23):
    """Draw boxes around detected cancer."""   
    fig,ax=plt.subplots(figsize=(8,10))
    ax.imshow(im,cmap='gray_r')
    inds = np.where(dets[:, -1] >= CONF_THRESH)[0]
    
    for i in inds:
        bbox = dets[i, :4]
        score = dets[i, -1]
        ax.add_patch(
            plt.Rectangle((bbox[0], bbox[1]),
                          bbox[2] - bbox[0],
                          bbox[3] - bbox[1], fill=False, linestyle ='dashed',
                          edgecolor=(0.95, 0.95, 0.5), linewidth=3))
    plt.axis('off')
    plt.tight_layout()
```