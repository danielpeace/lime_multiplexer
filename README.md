# lime_multiplexer

## Overview
Source code to accompany *Advancing Photonic Chip Design with Interpretable Machine Learning* by Pira et. al (https://doi.org/10.48550/arXiv.2505.09266).

Each notebook loads a series of 

A CNN is trained to classify simulated multiplexer designs by bandwidth and then uses LIME to explain which image regions drive the model’s decisions.

## 1) Input data
Inverse designed mode multiplexers used in the paper are contained within the `multiplexer_combined_bandwidths/` folder. The multiplexers are categorised into two folders two class folders: `combined_high_bw` and `combined_low_bw` based on their simulated tranmission bandwidth of both modes.

Bitmaps are read as grayscale images with pixel values normalised to `[0, 1]`, and resized **256×256** in size.

## 2) CNN

┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━┓
┃ Layer (type)                    ┃ Output Shape           ┃       Param # ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━┩
│ conv2d (Conv2D)                 │ (None, 254, 254, 32)   │           320 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ max_pooling2d (MaxPooling2D)    │ (None, 127, 127, 32)   │             0 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ conv2d_1 (Conv2D)               │ (None, 125, 125, 64)   │        18,496 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ max_pooling2d_1 (MaxPooling2D)  │ (None, 62, 62, 64)     │             0 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ conv2d_2 (Conv2D)               │ (None, 60, 60, 128)    │        73,856 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ max_pooling2d_2 (MaxPooling2D)  │ (None, 30, 30, 128)    │             0 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ flatten (Flatten)               │ (None, 115200)         │             0 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dense (Dense)                   │ (None, 128)            │    14,745,728 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dropout (Dropout)               │ (None, 128)            │             0 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dense_1 (Dense)                 │ (None, 2)              │           258 │
└─────────────────────────────────┴────────────────────────┴───────────────┘

![CNN](/docs/images/cnn.png)

## 3) Explanations
Running the LIME algorithm on the input data set will generate 'explanations', ie. which features after segmentation are most important for classifying input multiplexer designs into the two classes. 