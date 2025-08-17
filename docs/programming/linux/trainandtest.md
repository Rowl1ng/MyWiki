```
#!/usr/bin/env bash

for i in {1...10}
do
    sleep 2h
    cd Outputs/e2e_mask_rcnn_R-50-C4
    filename=`ls -t |head -n1|awk '{print $0}'`
    echo $filename

    cd $filename/ckpt
    ckpt=`ls -t |head -n1|awk '{print $0}'`
    echo $ckpt

    cd ../../../../
    CUDA_VISIBLE_DEVICES=7 python tools/test_net.py \
    --dataset gansu1 \
    --cfg configs/e2e_mask_rcnn_R-50-C4.yml \
    --load_ckpt Outputs/e2e_mask_rcnn_R-50-C4/$filename/ckpt/$ckpt

done
```