# easy-faster-rcnn.pytorch

An easy implementation of Faster R-CNN in PyTorch.


## Results

### Average Precision

| mAP  | aeroplane | bicycle | bird | boat | bottle |  bus  |  car  |  cat  | chair |  cow  | diningtable |  dog  | horse | motorbike | person | pottedplant | sheep | sofa | train | tvmonitor |
|:----:|:---------:|:-------:|:----:|:----:|:------:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----------:|:-----:|:-----:|:---------:|:------:|:-----------:|:-----:|:----:|:-----:|:---------:|
|0.7029|0.7133     |0.7814   |0.6815|0.5606|0.5272  |0.8188 |0.7919 |0.8296 |0.4973 |0.7729 |0.6799       |0.7945 |0.8018 |0.7647     |0.7655  |0.4244       |0.7152 |0.6748|0.7480 |0.7157     |

> mAP = 70.29%

### Speed (on GTX-1080-Ti)

* Training

    * **~7 examples** per seconds

    * **25 minutes** every 10000 steps

    * **3 hours** for 70000 steps (which leads to mAP=70%)

* Inference

    * **~9 examples** per seconds


## Requirements

* Python 3.6
* PyTorch 0.3.1
* tqdm

    ```
    $ pip install tqdm
    ```

## Setup

1. Download VOC 2007 Dataset
    - [Training / Validation](http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtrainval_06-Nov-2007.tar)
    - [Test](http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtest_06-Nov-2007.tar)

1. Extract to data folder, now your folder structure should be like:
    ```
    easy-faster-rcnn.pytorch
        - data
            - VOCdevkit
                - VOC2007
                    - Annotations
                        - 000001.xml
                        - 000002.xml
                        ...
                    - ImageSets
                        - Main
                            ...
                            test.txt
                            ...
                            trainval.txt
                            ...
                    - JPEGImages
                        - 000001.jpg
                        - 000002.jpg
                        ...
    ```

1. Build non-maximum-suppression module
    ```
    $ nvcc -arch=sm_61 -c --compiler-options -fPIC -o nms/src/nms_cuda.o nms/src/nms_cuda.cu
    $ python nms/build.py
    ```
    > sm_61 is for GTX-1080-Ti, to see others, visit [here](http://arnon.dk/matching-sm-architectures-arch-and-gencode-for-various-nvidia-cards/)


## Usage

1. Train

    ```
    $ python train.py --data_dir=./data --checkpoints_dir=./checkpoints
    ```

1. Evaluate

    ```
    $ python eval.py ./checkpoints/model-100.pth --data_dir=./data -r=./results
    ```

1. Clean

    ```
    $ rm -rf ./checkpoints
    $ rm -rf ./results
    ```
