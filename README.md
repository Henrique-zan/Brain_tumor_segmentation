# README — Brain Tumor Segmentation (BraTS 2020/2024)

🇺🇸 English | 🇧🇷 [Leia em Português](README.pt-BR.md)

---
## 1) Project Overview
This repository organizes all the code and artifacts used to evaluate the impact of different MRI sequences (T1n, T1c, T2w, T2f/FLAIR) in cross-dataset scenarios between BraTS 2020 and BraTS 2024.  

The structure was designed to:  
- **Create and visualize** dataset splits (train/val/test).  
- **Train** models (baseline and mixdataset) per sequence.  
- **Perform cross-dataset inference/evaluation** (train on A → test on B and vice versa) per sequence.  
- **Consolidate results** (global metrics and tumor-size quartile analysis) and display visual examples per sequence.

> In summary, the repository guides the complete workflow: data preparation → training → inference → analysis and final results table.

---

## 2) Datasets
- **BraTS 2020**: Dataset from the BraTS 2020 challenge, widely used in the literature. Available at:  
https://www.med.upenn.edu/cbica/brats2020/data.html  
or  
https://www.kaggle.com/datasets/awsaf49/brats2020-training-data  

- **BraTS 2024**: Dataset from the BraTS 2024 challenge, widely used in the literature. Available at:  
https://www.synapse.org/Synapse:syn53708249/wiki/626323  

> **Note:** Download the raw data, keep the original patient/sequence/mask folder structure, and preserve the original naming convention to ensure reproducibility.

> **Sequence naming equivalence:** There are naming differences between the datasets. To address this, we created a mapping for clarity:

```python
SEQ_KEYS_2024 = ["t1n","t1c","t2w","t2f","seg"]
SEQ_KEYS_2020 = ["t1","t1ce","t2","flair","seg"]
```

---

## 3) Project Structure

```
.
├─ .gitignore
├─ Artigo.pdf
├─ Poster.pdf
├─ README.md                                # English
├─ README.pt-BR.md                          # Portuguese
├─ environment_conda.yaml
├─ resultados.xlsx
├─ splits.pdf
├─ experimentos/
│  ├─ Dataset_viz_allslices_dataset_creation.ipynb
│  ├─ baseline2020_*.ipynb                  # 4 notebooks (one per sequence)
│  ├─ baseline2024_*.ipynb                  # 4 notebooks
│  ├─ crossdataset_20_24_*.ipynb            # 4 notebooks
│  ├─ crossdataset_24_20_*.ipynb            # 4 notebooks
│  ├─ mixdataset_full_2020_2024_*.ipynb     # 4 notebooks 
│  ├─ mixdataset2020_*.ipynb                # 4 notebooks 
│  ├─ mixdataset2024_*.ipynb                # 4 notebooks 
│  └─ modelos_treinados/                    # saved weights (.pt) per experiment
```

**Summary of the split/visualization notebook and the 28 experiment notebooks:**
- **Split / visualization** — 1 notebook (handles all sequences and experiments).  
- **Baseline** — 8 notebooks (4 sequences: train/test on 2020; 4 sequences: train/test on 2024).  
- **Cross-dataset** — 8 notebooks (4 sequences: train on 2020 → test on 2024; 4 sequences: train on 2024 → test on 2020).  
- **Mixdataset** — 12 notebooks (4 sequences: mixed training on 2020 + 2024; testing on 2020 + 2024 (Full) and separately on 2020 and 2024).

All experiments have self-explanatory names indicating scenario and sequence, following the structure: scenario - dataset - sequence.  
Trained weights are stored in `experimentos/modelos_treinados/`.

---

## 4) What Each Notebook Does
> **Note:** All notebooks have been executed, and all steps are documented, demonstrating the obtained results.

---

### 4.1) **Split and Visualization** Notebook
**File:** `Dataset_viz_allslices_dataset_creation.ipynb`

**Typical dataset loading parameters:**
```python
base_path_20 = r"C:\Users\dados\Downloads\path_brats2020"
base_path_24 = r"C:\Users\dados\Downloads\path_brats2024"
```

**Typical split output parameter:**
```python
VAR_CENARIO = r"D:\all_slices_dataset\split_a_ser_utilizado"
```

**Steps:**
- **Loading** BraTS 2020/2024 datasets.  
- **Visualization** of sample images per sequence and shape verification.  
- **Split creation** (70-15-15) by patient, saving into separate folders by split and sequence.  
- **Metadata creation** (CSV) per split and scenario.  
- **Image count** per split and test scenario.  
- **Duplicate patient verification** across splits to prevent data leakage.

---

### 4.2) **Training** Notebooks  
**Scenarios:** Baseline (2020, 2024) and Mixdataset_full  

**Typical parameters:**
```python
VAR_CENARIO = r"D:\all_slices_dataset\split_a_ser_utilizado"
SEQ = "sequence"    
MASK = "mask"
```

**Steps:**
- **Loading** image splits (train, validation, test).  
- **CUDA**: GPU connection/verification.  
- **Summary** of split sizes and shapes (images and masks).  
- **Plots** of image + mask + overlay.  
- **Path validation**.  
- **Model**: definition of **Res-UNet**.  
- **Metrics & Loss**: evaluation metrics and loss function definition.  
- **Training configuration** (epochs, learning rate, early stopping).  
- **Training loop**: saving weights and training logs.  
- **Unit test** + single example plot.  
- **Visual examples**: 2 best, 4 random, 2 worst predictions.  
- **Tumor size quartile analysis** (metrics per quartile).  
- **Full test evaluation** with computation of relevant metrics.

---

### 4.3) **Inference** Notebooks  
**Scenarios:** Crossdataset (2020→2024; 2024→2020) and Mixdataset (tests on 2020 and 2024)  

**Typical parameters:**
```python
VAR_CENARIO = r"D:\all_slices_dataset\split_a_ser_utilizado"
SEQ = "sequence"       
MASK = "mask"
```

**Steps:**
- **Loading** test split only.  
- **CUDA**: GPU connection/verification.  
- **Test split summary** and shape verification (images and masks).  
- **Plots** of image + mask + overlay.  
- **Path validation**.  
- **Model**: definition of **Res-UNet**.  
- **Metrics**: evaluation metric definition.  
- **Loading trained weights**.  
- **Unit test** + single example plot.  
- **Visual examples** (2 best, 4 random, 2 worst).  
- **Tumor size quartile analysis** (metrics per quartile).  
- **Full test evaluation** with computation of relevant metrics.

---

## 4.4) Key Variables (Paths and Sequence)

- **`VAR_CENARIO`**: `BASELINE_2020`, `BASELINE_2024`, `CROSS_2024_2020`, `CROSS_2020_2024`, `MIXDATASET_FULL`, `MIXDATASET_2020`, `MIXDATASET_2024`  
- **`SEQ`**: `"t1n"-"t1" | "t1c"-"t1ce" | "t2w"-"t2" | "t2f"-"flair"`  
- **`MASK`**: `"seg"` (standard name for all cases)

---

## 5) Notebook Reference Table

| Scenario        | Train on | Test on | Sequences (4)       | Nº notebooks |
|-----------------|----------|----------|--------------------|--------------|
| Baseline_2020   | 2020     | 2020     | T1n, T1c, T2w, T2f | 4            |
| Baseline_2024   | 2024     | 2024     | T1n, T1c, T2w, T2f | 4            |
| Cross_20→24     | 2020     | 2024     | T1n, T1c, T2w, T2f | 4            |
| Cross_24→20     | 2024     | 2020     | T1n, T1c, T2w, T2f | 4            |
| Mix_full        | 2020+2024| 2020+2024| T1n, T1c, T2w, T2f | 4            |
| Mix_2020        | 2020+2024| 2020     | T1n, T1c, T2w, T2f | 4            |
| Mix_2024        | 2020+2024| 2024     | T1n, T1c, T2w, T2f | 4            |

---

## 6) Trained Models
Weights (checkpoints) are saved in `experimentos/modelos_treinados/`, following the naming convention used in the notebooks, prefixed with `best`.

Example: `best_BASELINE2020_T1c_T1ce.pt`

---

## 7) Results Table and Split Illustration
The file `resultados.xlsx` contains a synthesis of all global evaluation metrics collected in the experiments.

The file `splits.pdf` provides a visual illustration of the tested scenarios in the project.

---

## 8) Research Paper and Presentation Poster
The file `Artigo.pdf` contains the development steps and conclusions of the work, presented in an academic and detailed format.

The file `Poster.pdf` summarizes the main points of the project in a concise and easy-to-read format.

---

## 9) Reproducibility
- Configure the environment using the `.yaml` file.  
- Download the BraTS 2020/2024 datasets.  
- Adjust path variables in the notebooks.  
- Run `Dataset_viz_allslices_dataset_creation.ipynb` to create splits and metadata.  
- Run the training notebooks to generate weights and test the respective scenarios.  
- Run the inference notebooks that use the trained weights to evaluate the scenarios.  
