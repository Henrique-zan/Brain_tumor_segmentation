# The Impact of MRI Sequences on Cross-Dataset Generalization of Brain Tumor Segmentation Models

This repository contains the code and resources for the research paper investigating the impact of different Magnetic Resonance Imaging (MRI) sequences on the generalization capabilities of deep learning models for brain tumor segmentation. The primary goal is to analyze how models trained on individual sequences (T1n, T1c, T2w, and T2f/FLAIR) perform when evaluated on a dataset different from the one used for training.

## 📖 About The Project

Automatic brain tumor segmentation is a critical step in diagnosis, surgical planning, and clinical follow-up. While deep learning architectures like U-Net and its variations have achieved significant success, their ability to generalize to new datasets (cross-dataset generalization) remains a major challenge. Differences in image acquisition protocols, signal intensity, and noise can drastically degrade a model's performance in real-world clinical settings.

This work provides a systematic analysis to isolate which individual MRI sequence offers the most robustness and generalization power.

### 🎯 Research Question

The central question guiding this research is:
> Which MRI sequences are most conducive to cross-dataset robustness in the binary segmentation of brain tumors?
## 🛠️ Methodology

To answer this question, a standardized pipeline was developed with the following components:

* **Architecture:** The model is based on a **2D ResUNet** architecture.
* **Datasets:**
    * **Training:** BraTS 2024 
    * **External Evaluation (Testing):** BraTS 2020
* **MRI Sequences Evaluated:** The model was trained independently for each of the four primary sequences:
    * T1n (native)
    * T1c (post-contrast)
    * T2w (T2-weighted)
    * T2f/FLAIR (Fluid-Attenuated Inversion Recovery)
* **Preprocessing:**
    * Using only slices that contain tumor regions.
    * Resizing images.
    * Combining multi-class segmentation masks into binary masks.
    * On-the-fly data augmentation using flips and rotations.

## ✅ Results

The experiments demonstrated that the model's robustness varies significantly depending on the MRI sequence used.

* The **T2f/FLAIR** sequence delivered the best and most stable performance in the cross-dataset scenario.
* The Dice coefficient for the T2f/FLAIR sequence remained consistently above **76%** during evaluation on the external dataset (BraTS 2020).

These findings suggest that in scenarios with limited computational resources or where not all modalities are available, the T2f/FLAIR sequence is the most reliable choice for ensuring model generalization.

## 🚀 How to Run the Project

**1. Prerequisites**

* Python 3.8+
* PyTorch
* SimpleITK
* NumPy
* etc.

**2. Installation**

Clone the repository:
```bash
git clone [https://github.com/Henrique-zan/Brain_tumor_segmentation.git](https://github.com/Henrique-zan/Brain_tumor_segmentation.git)
cd Brain_tumor_segmentation
```

Install the dependencies:
```bash
pip install -r requirements.txt
```

**3. Training**

To train the model on a specific sequence (e.g., FLAIR), run:
```bash
python train.py --dataset_path /path/to/brats2024 --sequence FLAIR
```

**4. Evaluation**

To evaluate the trained model on the test dataset, run:
```bash
python evaluate.py --model_path /path/to/trained_model.pth --test_dataset_path /path/to/brats2020
```

## 👨‍💻 Authors

* **Henrique Zan Grande** - [PUC-PR](https://www.pucpr.br/) 
* **João Gabriel Pitol** - [PUC-PR](https://www.pucpr.br/) 
* **Lucas Braga Schuck** - [PUC-PR](https://www.pucpr.br/)
* **Rafael Vargas Serenato** - [PUC-PR](https://www.pucpr.br/)
