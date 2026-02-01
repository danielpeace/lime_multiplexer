# lime_multiplexer

## Overview
This code applies local interpretable model-agnostic explanation (LIME) to a set of topologically inverse designed transverse mode multipelxers as presented *Advancing Photonic Chip Design with Interpretable Machine Learning* by Pira et. al (https://doi.org/10.48550/arXiv.2505.09266).

By applying the LIME algorithm to a small CNN trained to differentiate between two classes of high and low bandwidth multiplexers, we are able to use the LIME explanations to highlight particular regions within each class. Based on the identified regions we then modify the initial conditions of the inverse design optimisation in order to further improve the multiplexer bandwidths.

## 1) Input data
Inverse designed mode multiplexers used in the paper are contained within the `multiplexer_combined_bandwidths/` folder. The multiplexers are categorised into two folders two class folders: `combined_high_bw` and `combined_low_bw` based on their simulated tranmission bandwidth of both modes.

Bitmaps are read as grayscale images with pixel values normalised to `[0, 1]`, and resized **256×256** in size.

## 2) CNN
We train a simple sequential CNN using Adam optimiser with sparse categorical cross-entropy. The CNN is trained for 20 epochs using an 80/20 tran/test split.

**CNN Structure**
![CNN](/docs/images/cnn.png)

The final trained CNN model used in the paper is saved in [cnn_v4_combined_model_v1.h5](cnn_v4_combined_model_v1.h5) and can be loaded in the jupyter notebook.

**Model Accuracy**

![model_acc](/docs/images/cnn_v4_accuracy.png)


**Model Loss**

![model_loss](/docs/images/cnn_v4_loss.png)

## 3) LIME explanations
The LIME section:

- Loads the saved model (`cnn_v4_combined_model_v1.h5`).
- Defines a `predict_fn` wrapper that converts LIME’s RGB input back into grayscale tensors for the CNN.
- Uses `lime_image.LimeImageExplainer()` with SLIC superpixels to perturb images and estimate feature importance.
- Writes out:
  - Original images (`Original_*.png`)
  - LIME overlays (`LIME_Explanation_*.png`)
  - Raw binary/importance masks (`Mask_*.png`)

The helper functions `mask_average` and `lime_class_heatmaps` aggregate the per-image masks into **class-level heatmaps** to visualize which regions are generally important for each bandwidth class.

**Output Heatmap**

![heatmap](/docs/images/heatmap.png)