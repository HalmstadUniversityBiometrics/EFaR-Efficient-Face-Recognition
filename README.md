# EFaR-Efficient-Face-Recognition

This repository contains our submissions to [EFaR-2023, Efficient Face Recognition Competition](https://sites.google.com/view/ijcb-2023-efar/) at [IJCB 2023, the IEEE/IAPR International Joint Conference on Biometrics 2023](https://ijcb2023.ieee-biometrics.org/).

The models are an implementation of the **MobileNetv2 and SqueezeNet CNNs** (very **lightweight** architectures of just 8.15 and 4.55MB respectively) to carry out **face recognition**. They are trained on the MS-Celeb-1M and VGGFace2 databases (6.47M images in total). The networks have undergone a double fine-tuning, first over the MS1M-RetinaFace cleaned set (35k users/3.16M images, only users with more than 70 images), and then over VGGFace2 (9k users/3.31M images). Although VGG2 contains fewer users, it has more intra-class diversity due to more images per user. Due to this fact, the double fine-tuning strategy employed has shown increased performance compared to training the models only with one database, especially if it has few images per identity. This double fine-tuning implementation is suggested in the paper [VGGFace2: A dataset for recognising faces across pose and age](https://arxiv.org/abs/1710.08092).

<p align="center">
  <img src="https://github.com/HalmstadUniversityBiometrics/EFaR-Efficient-Face-Recognition/assets/6042693/54b13354-34a5-4f92-84d1-3adf068058c4" alt="EFAR2023"/>
</p>

The models are referred to as **HH-MB2 and HH-SQ** in the competition. We take ImageNet pre-trained networks as starting point. We have also added batch normalization between convolutions and ReLU layers of SqueezeNet, which are missing in its original implementation. Both CNNs have been modified to employ an input size of 113x113 by changing the stride of the first convolutional layer from 2 to 1. This allows to keep the rest of the network unchanged and to reuse ImageNet parameters as starting model. 

The networks are trained for biometric identification using the soft-max function and ImageNet as initialization. The optimizer is SGDM (mini-batch=128, learning rate=0.01, 0.005, 0.001, and 0.0001, decreased when the validation loss plateaus). Two percent of images per user in the training set are set aside for validation. We apply horizontal random flip during training. We train with Matlab r2022b and use the ImageNet pre-trained model that comes with such release.

After training, feature vectors for biometric verification are extracted from the layer adjacent to the classification layer (i.e., the Global Average Pooling). This corresponds to a descriptor of 1280 (HH-MB2) and 1000 (HH-SQ) elements.

# Requirements
  - Matlab software (tested in r2022b). We also provide the **onnx models** exported using the Matlab function "exportONNXNetwork".

# Usage
  - See readme_usage.txt file. A feature extraction script is also provided.

# Pre-processing
  - **Input images must be of 113 x 113**.
  - We follow the tight crop of VGGFace2, with an extra 30% increase of the face bounding box, so some contextual information is added aound the face. See attached examples to see how the input images should look like. The bounding box of VGG2 images is resized so the shortest side of the image has 129 pixels. Then, a 113x113 crop is taken.
  - The network is trained with images with some random displacement in horizontal and vertical dimensions (by taking a random 113x113 crop of the image during training), so it should be somehow tolerant to non-centered faces, althought it has not been tested (during testing, we only employ the center 113x113 crop of the bounding box). Bounding boxes provided with the VGG2 database are obtained using the MTCNN detector. See the VGGFace2 paper for more details.
