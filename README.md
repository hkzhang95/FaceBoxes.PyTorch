# FaceBoxes in PyTorch

[![License](https://img.shields.io/badge/license-BSD-blue.svg)](LICENSE)

Add support for user-specific datasets by [Hongkai Zhang](https://github.com/hkzhang95)

By [Zisian Wong](https://github.com/zisianw), [Shifeng Zhang](http://www.cbsr.ia.ac.cn/users/sfzhang/)

A [PyTorch](https://pytorch.org/) implementation of [FaceBoxes: A CPU Real-time Face Detector with High Accuracy](https://arxiv.org/abs/1708.05234). The official code in Caffe can be found [here](https://github.com/sfzhang15/FaceBoxes).

## What's new in my version?

- Fix a cython version bug when compiling NMS.
- Add support for user-specific datasets.
- Some useful tools such as visualizations, .gitignore, etc.

### Form your own dataset

Form your data like following:

```
data
├── dataset name
│   ├── images
│   ├── annotations
│   ├── img_list.txt
│   ├── test_img_list.txt
```

Each line in `img_list.txt` and `test_img_list.txt` should be like this:

```txt
dirname/imagename.jpg dirname/imagename.xml
```

Here the `dirname` does not contain `images/annotations`. In other words, the real image path is `images/dirname/imagename.jpg` and the annotation path is `annotations/dirname/imagename.jpg`.

Note: I also write the annotation path into `test_img_list.txt` for evaluation. You may replace it with something else since it is not required when testing.

### Train

- `--pin_memory` to speed up data loading (need large memory)

### Test

- `--resize ratio` to decide the ratio
- `-s` or `--show_image` to visualize the detection results
- `--vis_thres thres` to show the results with scores >= `thres`

### Evaluation

You can evaluate the performance like follows (only `AP50` now):

```Shell
python tools/eval_voc_ap.py path/to/detfile path/to/img_list dataset_name
```

## Performance
| Dataset | Original Caffe | PyTorch Implementation |
|:-|:-:|:-:|
| AFW | 98.98 % | 98.47% |
| PASCAL | 96.77 % | 96.84% |
| FDDB | 95.90 % | 95.44% |

## Citation
Please cite the paper in your publications if it helps your research:

    @inproceedings{zhang2017faceboxes,
      title = {Faceboxes: A CPU Real-time Face Detector with High Accuracy},
      author = {Zhang, Shifeng and Zhu, Xiangyu and Lei, Zhen and Shi, Hailin and Wang, Xiaobo and Li, Stan Z.},
      booktitle = {IJCB},
      year = {2017}
    }

### Contents
- [Installation](#installation)
- [Training](#training)
- [Evaluation](#evaluation)
- [References](#references)

## Installation
1. Install [PyTorch](https://pytorch.org/) >= v1.0.0 following official instruction.

2. Clone this repository. We will call the cloned directory as `$FaceBoxes_ROOT`.
```Shell
git clone https://github.com/hkzhang95/FaceBoxes.PyTorch.git
```

3. Compile the nms:
```Shell
./make.sh
```

_Note: Codes are based on Python 3+._

## Training
1. Download [WIDER FACE](http://mmlab.ie.cuhk.edu.hk/projects/WIDERFace/index.html) dataset, place the images under this directory:
  ```Shell
  $FaceBoxes_ROOT/data/WIDER_FACE/images
  ```
2. Convert WIDER FACE annotations to VOC format or download [our converted annotations](https://drive.google.com/open?id=1-s4QCu_v76yNwR-yXMfGqMGgHQ30WxV2), place them under this directory:
  ```Shell
  $FaceBoxes_ROOT/data/WIDER_FACE/annotations
  ```

3. Train the model using WIDER FACE:
  ```Shell
  cd $FaceBoxes_ROOT/
  python3 train.py
  ```

If you do not wish to train the model, you can download [our pre-trained model](https://drive.google.com/open?id=128m1QasIwQRkrY-Eb5Epi-ShXnrZWUCQ) and save it in `$FaceBoxes_ROOT/weights`.


## Evaluation
1. Download the images of [AFW](https://drive.google.com/open?id=1Kl2Cjy8IwrkYDwMbe_9DVuAwTHJ8fjev), [PASCAL Face](https://drive.google.com/open?id=1p7dDQgYh2RBPUZSlOQVU4PgaSKlq64ik) and [FDDB](https://drive.google.com/open?id=17t4WULUDgZgiSy5kpCax4aooyPaz3GQH) to:
```Shell
$FaceBoxes_ROOT/data/AFW/images/
$FaceBoxes_ROOT/data/PASCAL/images/
$FaceBoxes_ROOT/data/FDDB/images/
```

2. Evaluate the trained model using:
```Shell
# dataset choices = ['AFW', 'PASCAL', 'FDDB']
python3 test.py --dataset FDDB
# evaluate using cpu
python3 test.py --cpu
```

3. Download [eval_tool](https://bitbucket.org/marcopede/face-eval) to evaluate the performance.

## References
- [Official release (Caffe)](https://github.com/sfzhang15/FaceBoxes)
- A huge thank you to SSD ports in PyTorch that have been helpful:
  * [ssd.pytorch](https://github.com/amdegroot/ssd.pytorch), [RFBNet](https://github.com/ruinmessi/RFBNet)

  _Note: If you can not download the converted annotations, the provided images and the trained model through the above links, you can download them through [BaiduYun](https://pan.baidu.com/s/1HoW3wbldnbmgW2PS4i4Irw)._
