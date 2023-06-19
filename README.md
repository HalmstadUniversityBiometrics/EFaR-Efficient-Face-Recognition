# EFaR-Efficient-Face-Recognition

This repository contains our submissions to [EFaR-2023, Efficient Face Recognition Competition](https://sites.google.com/view/ijcb-2023-efar/) at [IJCB 2023, the IEEE/IAPR International Joint Conference on Biometrics 2023](https://ijcb2023.ieee-biometrics.org/).

The models are an implementation of the MobileNetv2 and SqueezeNet CNNs (very lightweight architectures of just 8.15 and 4.55MB respectively) to carry out face recognition. They are trained on the MS-Celeb-1M and VGGFace2 databases (6.47M images in total). The training is done following the double fine-tuning implementation suggested in the paper [VGGFace2: A dataset for recognising faces across pose and age](https://arxiv.org/abs/1710.08092).

The models are referred to as HH-MB2 and HH-SQ in the competition. We take ImageNet pre-trained networks as starting point. We have also added batch normalization between convolutions and ReLU layers of SqueezeNet, which are missing in its original implementation. Both CNNs have been modified to employ an input size of 113$\times$113$\times$3 by changing the stride of the first convolutional layer from 2 to 1. This allows to keep the rest of the network unchanged and to reuse ImageNet parameters as starting model. 
