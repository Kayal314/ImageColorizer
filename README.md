# Deep Learning Model that Colors Grayscale Images

Main Paper by Zezhou Cheng, Qingxiong Yang, Bin Sheng: [Deep Colorization](https://arxiv.org/abs/1605.00075)

## Some Predicted Results
<h4> Test Images </h4>
<p align=center><img src='ReadmeTestImages/testimage1.jpg' alt='Test Image 1'> <img src='ReadmeTestImages/testimage2.jpg' alt='Test Image 2'>
<img src='ReadmeTestImages/testimage3.jpg' alt='Test Image 3'></p>
<h4> Generated Images </h4>
<p align=center><img src='Results/predimage1.jpg' alt='Predicted Image 1'> <img src='Results/predimage11.jpg' alt='Predicted Image 2'>
<img src='Results/predimage14.jpg' alt='Predicted Image 3'></p>

## Network Architecture

![Network Architecture](NetworkArchitecture.png)

<h4> Encoder Network Architecture </h4>

| Layer | Filters       | Kernel Size   | Strides | Padding | Activation |
|:---------:|:-------------:|:-------------:|:-----:|:-------:|:----------:|
| Conv2D_E1 | 64      | (3 × 3) | (2 × 2) | same | ReLU |
| Conv2D_E2 | 128     | (3 × 3) | (1 × 1) | same | ReLU |
| Conv2D_E3 | 128     | (3 × 3) | (2 × 2) | same | ReLU |
| Conv2D_E4 | 256     | (3 × 3) | (1 × 1) | same | ReLU |
| Conv2D_E5 | 256     | (3 × 3) | (2 × 2) | same | ReLU |
| Conv2D_E6 | 512     | (3 × 3) | (1 × 1) | same | ReLU |
| Conv2D_E7 | 512     | (3 × 3) | (1 × 1) | same | ReLU |
| Conv2D_E8 | 256     | (3 × 3) | (1 × 1) | same | ReLU |

<h4> Fusion Network Architecture </h4>

| Layer | Filters       | Kernel Size   | Strides | Padding | Activation |
|:---------:|:-------------:|:-------------:|:-----:| :-------:|:----------:|
| Conv2D_F1| 256      | (1 × 1) | (1 × 1) | same | ReLU |

<h4> Decoder Network Architecture </h4>

| Layer | Filters       | Kernel Size   | Strides | Padding | Activation |
|:---------:|:-------------:|:-------------:|:-----:|:-------:|:----------:|
| Conv2D_D1 | 128     | (3 × 3) | (1 × 1) | same | ReLU |
| UpSamp2D_D1 | -     | - | - | - | - |
| Conv2D_D2 | 64     | (3 × 3) | (1 × 1) | same | ReLU |
| Conv2D_D3 | 64     | (3 × 3) | (1 × 1) | same | ReLU |
| UpSamp2D_D2 | -     | - | - | - | - |
| Conv2D_D4 | 32    | (3 × 3) | (1 × 1) | same | ReLU |
| Conv2D_D5 | 2     | (3 × 3) | (1 × 1) | same | tanh |
| UpSamp2D_D2 | -     | - | - | - | - |


## Fusion Layer Architecture

![Fusion Layer](FusionLayer.png)

## High-Level Feature Extraction through Inception Resnet v2

The Inception Resnet v2 Model extracts the high-level features of the input grayscale image. The last layer before the softmax activation outputs a vector of size 1000 or dimension (1000 × 1 × 1) (feature-vector). This vector is repeated 28 × 28 times and then reshaped into a volume of (28 × 28 × 1000). This volume is then concatenated depth-wise to the Conv2D_E8 layer. This whole block of size (28 * 28 * 1256) is then passed through Conv2D_F1.


