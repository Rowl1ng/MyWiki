# 可视化

```
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