# Dolly Zoom

## Introduction
We aim to create a dolly zoom effect on a single shot without any depth information.

In Dolly Zoom, the foreground stays the same while the background moves, so we need to have depth and matting information for the shot.

We used 3D ken burn effect as our baseline. We upgraded the depth estimation block to "Boosting Monocular Depth Estimation Models to High-Resolution via Content-Adaptive Multi-Resolution Merging". 

Here is our pipeline:

Given a sequence of video frames, we generate depth and trimap estimations, we then fuse the trimap and depth map and generate a refined depth map. 
We give in our refined depth map and our input source frames to our view synthesis network to generate the final result.

![841F9BD9-4A6D-4F81-BE3D-34A69F848A15](https://github.com/sepideh-srj/dollyzoom/assets/12370175/2776e22f-a63d-407a-a3ab-abb0ac628ce9)
![B40AAB41-AF2A-4827-A6C6-127B4B6E532A](https://github.com/sepideh-srj/dollyzoom/assets/12370175/0cf3658e-e7cc-4fee-be81-67e99340eb44)


## setup
Tested with Python 3.6 and Pytorch 1.6. 

Several functions are implemented in CUDA using CuPy, which is why CuPy is a required dependency. It can be installed using `pip install cupy` or alternatively using one of the provided [binary packages](https://docs.cupy.dev/en/stable/install.html#installing-cupy) as outlined in the CuPy repository. Please also make sure to have the `CUDA_HOME` environment variable configured.

In order to generate the video results, please also make sure to have `pip install moviepy` installed.

Three different Repositories are used in this repo.

Download Midas model weights from https://github.com/intel-isl/MiDaS. Put the weight in the following path : 
```
midas/model-f46da743.pt
```
Download depthmerge model weights from https://github.com/ouranonymouscvpr/cvpr2021_ouranonymouscvpr. Put the weights in the following path :
```
depthmerge/checkpoints/scaled_04_1024/latest_net_G.pth
```

## Usage
To run it on a video and generate the Vertigo effect (Dolly Zoom) fully automatically, use the following command.

first edit the following three lines of dollyzoom.py
```
    arguments_strIn = ['./images/input.mp4']
    arguments_strOut = './output'
    starter_zoom = 2
```
Then run
```
python dollyzoom.py'
```
## Results
The original video is on the left, and the final result with the dolly zoom effect is on the right.



https://github.com/sepideh-srj/dollyzoom/assets/12370175/3b29b5e8-01cb-4d3b-ad7c-c1dac7911953


https://github.com/sepideh-srj/dollyzoom/assets/12370175/a167e7a7-b355-45c3-9c9c-025ee274d5cc









https://github.com/sepideh-srj/dollyzoom/assets/12370175/3911cb23-c4f8-43ab-954d-b6f0311f4920




## Acknowledgement
We borrowed some parts of the following papers and their implementation for our project

### Midas
https://github.com/compphoto/BoostingMonocularDepth
```
@INPROCEEDINGS{Miangoleh2021Boosting,
author={S. Mahdi H. Miangoleh and Sebastian Dille and Long Mai and Sylvain Paris and Ya\u{g}{\i}z Aksoy},
title={Boosting Monocular Depth Estimation Models to High-Resolution via Content-Adaptive Multi-Resolution Merging},
journal={Proc. CVPR},
year={2021},
}
```
https://github.com/intel-isl/MiDaS
```
@article{Ranftl2020,
	author    = {Ren\'{e} Ranftl and Katrin Lasinger and David Hafner and Konrad Schindler and Vladlen Koltun},
	title     = {Towards Robust Monocular Depth Estimation: Mixing Datasets for Zero-shot Cross-dataset Transfer},
	journal   = {IEEE Transactions on Pattern Analysis and Machine Intelligence (TPAMI)},
	year      = {2020},
}
```

### 3D ken Burns effect from a single image
https://github.com/sniklaus/3d-ken-burns
```
@article{Niklaus_TOG_2019,
         author = {Simon Niklaus and Long Mai and Jimei Yang and Feng Liu},
         title = {3D Ken Burns Effect from a Single Image},
         journal = {ACM Transactions on Graphics},
         volume = {38},
         number = {6},
         pages = {184:1--184:15},
         year = {2019}
     } 
```
### Pix2Pix 
https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix
```
@inproceedings{CycleGAN2017,
  title={Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networkss},
  author={Zhu, Jun-Yan and Park, Taesung and Isola, Phillip and Efros, Alexei A},
  booktitle={Computer Vision (ICCV), 2017 IEEE International Conference on},
  year={2017}
}


@inproceedings{isola2017image,
  title={Image-to-Image Translation with Conditional Adversarial Networks},
  author={Isola, Phillip and Zhu, Jun-Yan and Zhou, Tinghui and Efros, Alexei A},
  booktitle={Computer Vision and Pattern Recognition (CVPR), 2017 IEEE Conference on},
  year={2017}
}
```
### MODNet: Is a Green Screen Really Necessary for Real-Time Portrait Matting? 
https://github.com/ZHKKKe/MODNet/blob/master/README.md
```
@article{MODNet,
  author = {Zhanghan Ke and Kaican Li and Yurou Zhou and Qiuhua Wu and Xiangyu Mao and Qiong Yan and Rynson W.H. Lau},
  title = {Is a Green Screen Really Necessary for Real-Time Portrait Matting?},
  journal={ArXiv},
  volume={abs/2011.11961},
  year = {2020},
}
```


