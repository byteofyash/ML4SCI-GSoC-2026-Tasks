# ML4SCI GSoC 2026: General & Image-Based Tests (EXXA)

**Applicant Name:** Yash Ranjan
 
**Target ML4SCI Projects:** EXXA2

## Overview
This repository contains my test task submission for the ML4SCI GSoC 2026 program. Because both the **General Test** (Unsupervised Clustering) and the **Image-Based Test** (Autoencoder) utilize the same ALMA synthetic continuum observations dataset, I have designed a unified, end-to-end deep learning pipeline. 

The Convolutional Autoencoder serves as a powerful feature extractor (solving Task 2), and its accessible 64-dimensional latent space is directly leveraged for K-Means clustering to detect protoplanetary disk signatures (solving Task 1).

## Repository Contents
```text
ML4SCI-GSoC-2026-Tasks/
├── Task_1_and_Task_2/
│   ├── solution.ipynb (Automated end-to-end pipeline)
│   └── solution.pdf (Exported notebook for immediate visual review)
├── outputs/ 
│   ├── preprocessed_sample.png
│   ├── pipeline_architecture.png
│   ├── autoencoder_reconstructions.png (Task 2)
│   ├── tsne_latent_space_clusters.png (Task 1)
│   └── cluster_samples_gallery.png (Task 1)
├── requirements.txt
└── README.md
```

## Deep Learning Methodology & Results

### 1. Data Preprocessing
The `.fits` files are loaded using `astropy`. I extract the 0th index layer (1250 microns continuum), apply a square root stretch to highlight faint astronomical features, and resize the data to 128x128 for optimal processing.

### 2. Task 2: Image-Based Autoencoder
I developed a Convolutional Autoencoder to compress the images into a dense latent representation and reconstruct them.

- **Architecture:** The Encoder uses 4 convolutional layers (with LeakyReLU) compressing the input down to an accessible 64-dimensional latent space. The Decoder uses transposed convolutions to upscale back to 128x128.
- **Quantitative Metrics:** Evaluated on withheld data, the model achieved an **Average MSE Loss of 0.001120** and an **Average MS-SSIM Score of 0.9204**, demonstrating highly accurate structural reconstructions.
- **Accessible Latent Space:** The pipeline includes a dedicated function to easily pass any new disk image through the encoder to access its 64D latent vector for downstream analysis.

### 3. Task 1: Unsupervised Clustering (General Test)
Rather than clustering on raw pixels, I used the autoencoder's latent space to group the disks by physical properties rather than superficial viewing angles.

- **Methodology:** Silhouette scores identified `k=2` as the optimal number of clusters. K-Means clustering was then applied to the 64-dimensional latent representations.
- **Qualitative Analysis (Planetary Signatures):** 
  - **Cluster 0:** Successfully groups continuous, diffuse disks without prominent gaps.
  - **Cluster 1:** Successfully isolates disks exhibiting clear planetary signatures, such as distinct rings and deep gaps carved by forming planets. 
  - The model successfully learned to ignore viewing angle/inclination, clustering purely on the presence or absence of structural planetary markers.

## Running the Code
The pipeline is entirely automated. The `solution.ipynb` script will automatically download the dataset and the pre-trained model weights via `gdown`, preprocess the data, run inferences, calculate metrics, and generate all visualizations (t-SNE plots, reconstruction galleries) from start to finish without any end-user modification required.


**Pre-trained Model Weights:** 
To comply with GitHub storage limits and the task deliverables, the pre-trained `exxa_autoencoder_weights.pth` file is hosted externally and is automatically downloaded during the script execution. 

You can also view/download it directly [here](https://drive.google.com/file/d/12D37JdYxnHpCejwyCVI2354RP8xPZgvJ/view?usp=sharing). 

