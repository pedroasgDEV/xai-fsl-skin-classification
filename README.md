# Few-Shot Learning for Cross-Domain Skin Lesion Classification

## Overview

This project is a reproduction and extension of the research paper **"Few-Shot Learning for Skin Cancer Classification"** (Chamarthi et al., 2024).

The goal is to classify skin lesions (Melanoma vs. Benign Nevus) in a **Few-Shot Learning (FSL)** setting, specifically tackling the **Domain Shift** problem (e.g., training on images from Austria and testing on images from Spain/USA).

### Key Differentiator

Unlike the original paper, this project integrates **Explainable AI (XAI)** using **Grad-CAM**. This allows us to visualize the model's attention maps, ensuring that predictions are based on the lesion morphology rather than artifacts (e.g., rulers, hair, or dark corners).

---

## Methodology

### 1. Architecture

* **Backbone:** ResNet50 (Pre-trained on ImageNet).
* **FSL Algorithm:** Prototypical Networks (ProtoNets).
* Computes a "prototype" (mean embedding) for each class in the metric space.
* Classification is performed by finding the nearest prototype using Euclidean distance.



### 2. Experimental Setup (N-Way, K-Shot)

* **Support Set:** K labeled images per class used to build the prototypes.
* **Query Set:** Unlabeled images to be classified.
* **Setting:** 2-Way (Melanoma/Nevus), 5-Shot (typically).

### 3. Explainability

* **Grad-CAM:** Applied to the last convolutional layer of the ResNet backbone to generate heatmaps highlighting the regions of interest.

---

## Dataset & Domain Shift

We utilize the **ISIC Archive** datasets, organized to simulate real-world domain shifts (different hospitals, cameras, and lighting conditions).

Data was sourced using the [DLR-DW ISIC Downloader](https://gitlab.com/dlr-dw/isic_download).

| Dataset | Role | Source | Characteristics |
| --- | --- | --- | --- |
| **HAM10000** | **Source Domain (Train)** | Vienna/Australia | "Standard" patients. Used for training the backbone. |
| **BCN20000** | **Target Domain (Test)** | Barcelona (Spain) | Different camera sensors. High difficulty. |
| **MSK** | **Target Domain (Test)** | New York (USA) | Different lighting. High difficulty. |

> **Preprocessing:** All images are resized to `224x224`, center-cropped to remove vignetting (black corners), and normalized using ImageNet statistics.

---

## Project Structure

```text
/
├── datasets/            # HAM10000, BCN, MSK
├── metadata/            # Contains .csv files of the datasets
├── lib/                 # Core logic scripts
│   ├── configs.py       # Configuration and Path management
│   ├── dataset.py       # Custom PyTorch DataLoader
│   ├── model.py         # ResNet + ProtoNet implementation
│   └── utils.py         # Helper functions (GradCAM, Metrics)
├── notebooks/           # Jupyter Notebooks for experiments
├── requirements.txt     # Python dependencies
└── README.md            # This file

```

## Installation

1. Clone this repository:
```bash
git clone https://github.com/pedroasgDEV/xai-fsl-skin-classification.git
cd xai-fsl-skin-classification

```

2. Install dependencies:
```bash
pip install -r lib/requirements.txt

```

3. **Data Setup:**
* Place the `HAM10000.zip`, `BCN.zip`, and `MSK.zip` files into the `datasets/` folder.
* Place the CSV metadata files into the `metadata/` folder.


## References

1. **Original Paper:** Chamarthi, R. et al. (2024). *Cross-domain few-shot learning for skin lesion classification*. Informatics in Medicine Unlocked. [Link](https://www.sciencedirect.com/science/article/pii/S2352914824000765)
2. **Dataset Source:** DLR-DW ISIC Download. [GitLab](https://gitlab.com/dlr-dw/isic_download)
3. **Prototypical Networks:** Snell, J., Swersky, K., & Zemel, R. (2017). *Prototypical Networks for Few-shot Learning*.

