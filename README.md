# lime_multiplexer

## Overview
Source code to accompany *Advancing Photonic Chip Design with Interpretable Machine Learning* by Pira et. al (https://doi.org/10.48550/arXiv.2505.09266).

Each notebook loads a series of 

A CNN is trained to classify simulated multiplexer designs by bandwidth and then uses LIME to explain which image regions drive the model’s decisions.

## 1) Input data
Inverse designed mode multiplexers used in the paper are contained within the `multiplexer_combined_bandwidths/` folder. The multiplexers are categorised into two folders two class folders: `combined_high_bw` and `combined_low_bw` based on their simulated tranmission bandwidth of both modes.

Bitmaps are read as grayscale images with pixel values normalised to `[0, 1]`, and resized **256×256** in size.

## 2) CNN
We train a simple sequential CNN using Adam optimiser with sparse categorical cross-entropy. The CNN is trained for 20 epochs using an 80/20 tran/test split.

**CNN Structure**
![CNN](/docs/images/cnn.png)

The final trained CNN model used in the paper is saved in [text](cnn_v4_combined_model_v1.h5) and can be loaded in the jupyter notebook.

Training history for the CNN.

**Model Accuracy**
![CNN](/docs/images/cnn_v4_accuracy.png)


**Model Loss**
![CNN](/docs/images/cnn_v4_loss.png)

## 3) Explanations
Running the LIME algorithm on the input data set will generate 'explanations', ie. which features after segmentation are most important for classifying input multiplexer designs into the two classes. 