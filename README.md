# 🚀 CARE2026 3D LiFS Inference Pipeline

![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white)
![Challenge](https://img.shields.io/badge/CARE-2026_Liver_Challenge-blue?style=for-the-badge)

> A unified registration-guided 3DINO framework for robust liver fibrosis stage prediction (S1-S4) under missing MRI modalities.

This repository contains the official 3D inference pipeline for the **CARE 2026 Liver Challenge**. The method leverages a frozen 3DINO feature extractor and a 4-fold ensemble classifier to generate highly calibrated per-case probabilities, ensuring robust performance even with severe modality missingness.

---

## ✨ Key Features

*   🧠 **3DINO Feature Extraction:** Captures continuous three-dimensional spatial topologies, overcoming the limitations of 2D slice-by-slice processing.
*   🧩 **Missing Modality Resilience:** Autonomously handles up to 7 MRI channels (`T1`, `T2`, `DWI`, `GED1`, `GED2`, `GED3`, `GED4`). Missing modalities are dynamically imputed using global mean features.
*   ⚖️ **Threshold-Calibrated Ensemble:** Utilizes a 4-fold classifier ensemble mapped with a calibrated ordered-sigmoid function to resolve subtle fibrosis stage transitions.

---

## ⚙️ Pipeline Overview

Our inference workflow follows a strict, automated pipeline:

1.  **Registration:** Register all available target modalities to the universal `GED4` anchor.
2.  **Mask Warping:** Resample the `GED4` mask into the registered image space.
3.  **Patch Extraction:** Crop around the liver mask and extract 3D patches after applying per-volume z-score normalization.
4.  **Tokenization:** Extract 3DINO patch tokens for each available modality.
5.  **Imputation:** Fill truly missing modality features with training-set global mean features.
6.  **Ensemble Inference:** Run the 4-fold classifier ensemble and average the fold-level `class_4_percentage` scores.
7.  **Probability Mapping:** Convert the ensemble score into final S1-S4 probabilities using calibrated thresholds ($t_{12}, t_{23}, t_{34}$).

---

## 📂 Repository Layout

```text
.
├── 🐳 Dockerfile
├── 🐍 main.py
├── 🐍 3D_validation.py
├── 📜 test.sh
├── 📦 requirements.txt
├── ⚙️ arg_config.py
├── 🧠 model_class.py
├── 🏃 trainer.py
├── 📁 Data_Prepare/
├── 📁 3DINO/
└── 📁 model/
