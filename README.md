# lime_multiplexer

## Overview
Source code to accompany Advancing Photonic Chip Design with Interpretable Machine Learning paper.

A CNN is trained to classify simulated multiplexer designs by bandwidth and then uses LIME to explain which image regions drive the model’s decisions.

## 1) Input data

- `multiplexer_combined_bandwidths/`
  - Dataset directory with two class folders: `combined_high_bw` and `combined_low_bw`.

## 2) CNN

- 3 convolution + max-pooling blocks
- Flatten + dense layer (128 units) with dropout
- Final 2-class softmax output


## 3) Explanations
Running the LIME algorithm on the input data set will generate 'explanations', ie. which features after segmentation are most important for classifying input multiplexer designs into the two classes. 