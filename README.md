<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1a0a2e,100:0d1117&height=200&section=header&text=IEEE-CIS%20Fraud%20Detection&fontSize=38&fontColor=a78bfa&animation=fadeIn&fontAlignY=38&desc=High-Performance%20Feature%20Engineering%20Pipeline&descAlignY=58&descSize=16&descColor=8b949e"/>

<img src="https://img.shields.io/badge/Competition-IEEE--CIS%20Fraud%20Detection-a78bfa?style=for-the-badge&labelColor=0d1117"/>
<img src="https://img.shields.io/badge/Records-590K%2B%20Transactions-58a6ff?style=for-the-badge&labelColor=0d1117"/>
<img src="https://img.shields.io/badge/Memory%20Saved-~60%25-7ee787?style=for-the-badge&labelColor=0d1117"/>
<img src="https://img.shields.io/badge/Platform-Ubuntu%20Linux-ff7b72?style=for-the-badge&labelColor=0d1117"/>

</div>

---

## 📌 Problem Statement

Financial fraud detection is one of the most critical applications of machine learning. The **IEEE-CIS Fraud Detection** competition presented a real-world challenge: given highly anonymized financial transaction data with extreme class imbalance and massive feature sparsity, build a pipeline capable of preparing raw data into a clean, consistent, and model-ready format.

This repository delivers that pipeline — a production-grade preprocessing and feature engineering system designed to handle over **590,000 training transactions** joined with identity records.

---

## 🗂️ Dataset Overview

| Property | Details |
|---|---|
| Source | [Kaggle — IEEE-CIS Fraud Detection (2018)](https://www.kaggle.com/c/ieee-fraud-detection) |
| Training transactions | 590,540 rows |
| Test transactions | 506,691 rows |
| Total features (raw) | 433 columns |
| Identity table rows | ~144,000 (join on TransactionID) |
| Raw dataset size | ~6 GB |
| Fraud rate | ~3.5% (severe class imbalance) |

> ⚠️ The raw dataset is excluded from this repo due to GitHub's 100MB file limit. See [Dataset Access](#-dataset-access) below.

---

## 🧠 Pipeline Architecture

```text
Raw Data (6GB)
|
▼
┌─────────────────────────────────┐
| 1. Relational Merge             |
| train_transaction +             |
| train_identity → unified df     |
└─────────────────────────────────┘
|
▼
┌─────────────────────────────────┐
| 2. Memory Optimization          |
| dtype downcasting               |
| float64 → float32               |
| int64 → int8/16/32              |
| Result: ~60% memory reduction   |
└─────────────────────────────────┘
|
▼
┌─────────────────────────────────┐
| 3. Feature Engineering          |
| • Frequency encoding (addr1)    |
| • Label encoding (categoricals) |
| • Group-aware scaling           |
|  C-features → scaler_c          |
|  D-features → scaler_d          |
|  V-features → scaler_v          |
|  All amounts → min_max_scaler   |
└─────────────────────────────────┘
|
▼
┌─────────────────────────────────┐
| 4. Inference Consistency        |
| All encoders/scalers saved      |
| as .pkl → applied identically   |
| to test set                     |
└─────────────────────────────────┘
|
▼
ML-Ready Dataset ✅
```
---

## ⚙️ Key Technical Decisions

### 1. Memory Optimization
Downcasting numeric dtypes reduced memory usage by approximately **60%**, making it possible to process the full 6GB dataset on standard hardware without running out of RAM.

### 2. Group-Aware Scaling
Rather than applying a single scaler across all features, different feature groups were scaled separately — matching the IEEE-CIS feature naming convention:

| Scaler | Feature Group | Description |
|---|---|---|
| `scaler_c.pkl` | C1–C14 | Counting features |
| `scaler_d.pkl` | D1–D15 | Timedelta features |
| `scaler_v.pkl` | V1–V339 | Vesta-engineered features |
| `min_max_scaler.pkl` | TransactionAmt | Raw transaction amounts |

### 3. Frequency Encoding
Instead of label-encoding high-cardinality geographic features like `addr1`, **frequency encoding** was used — replacing each value with how often it appears in the dataset. This preserves statistical signal without creating arbitrary ordinal relationships.
Saved as `addr1_counts.pkl`.

### 4. Inference Consistency
Every transformation object (encoders, scalers, frequency maps) is persisted as a `.pkl` file and applied **identically** to the test notebook. This is a production-grade pattern that eliminates data leakage and ensures reproducibility.

---

## 📊 Pipeline Results

| Metric | Value |
|---|---|
| Memory reduction | ~60% via dtype downcasting |
| Features processed | 433 raw → clean numeric matrix |
| Saved artifacts | 5 `.pkl` files (encoders + scalers) |
| Fraud class ratio | 3.5% positive / 96.5% negative |
| Notebooks | Train pipeline + Test pipeline (separate) |

---
## 📁 Repository Structure

```text
fraud-detection-feature-engineering-pipeline/
│
├── Train_Data_Preprocessing.ipynb   # Full training pipeline
├── Test_Data_Preprocessing.ipynb    # Inference pipeline (reuses .pkl artifacts)
├── Train_Data_Preprocessing.pdf     # Notebook export with all outputs
├── Test_Data_Preprocessing.pdf      # Notebook export with all outputs
│
├── label_encoders_dict.pkl          # Categorical → numeric mappings
├── min_max_scaler.pkl               # Scaler for TransactionAmt
├── scaler_c.pkl                     # Scaler for C-feature group
├── scaler_d.pkl                     # Scaler for D-feature group
├── scaler_v.pkl                     # Scaler for V-feature group
├── addr1_counts.pkl                 # Frequency map for addr1 encoding
│
├── .gitignore                       # Excludes 6GB+ raw CSVs
└── README.md
```
---

## 🚀 How to Run

### 1. Get the Dataset
Download from Kaggle: [IEEE-CIS Fraud Detection](https://www.kaggle.com/c/ieee-fraud-detection/data)

Place these files in your project folder:
- train_transaction.csv
- train_identity.csv
- test_transaction.csv
- test_identity.csv



### 2. Install Dependencies
```bash
pip install pandas numpy scikit-learn matplotlib
```

### 3. Run the Notebooks in Order
```bash
# Step 1 — builds all .pkl artifacts
jupyter notebook Train_Data_Preprocessing.ipynb

# Step 2 — applies saved artifacts to test set
jupyter notebook Test_Data_Preprocessing.ipynb
```

> ⚠️ Always run the Train notebook first. The Test notebook depends on the `.pkl` files it generates.

---

## 🛠️ Tech Stack

<div align="center">
  <img src="https://img.shields.io/badge/Python-3.x-3776ab?style=flat-square&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?style=flat-square&logo=pandas&logoColor=white"/>
  <img src="https://img.shields.io/badge/NumPy-Numerical-013243?style=flat-square&logo=numpy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Scikit--Learn-ML%20Pipeline-f7931e?style=flat-square&logo=scikit-learn&logoColor=white"/>
  <img src="https://img.shields.io/badge/Pickle-Artifact%20Persistence-58a6ff?style=flat-square"/>
  <img src="https://img.shields.io/badge/Ubuntu-Linux-E95420?style=flat-square&logo=ubuntu&logoColor=white"/>
</div>

---

## 🔭 What's Next

This pipeline produces a clean, normalized, ML-ready dataset. The natural next step is model training:

- [ ] Train a baseline with **LightGBM** or **XGBoost**
- [ ] Address class imbalance with **SMOTE** or class weighting
- [ ] Evaluate using **AUC-ROC** (the competition metric)
- [ ] Experiment with threshold tuning for precision/recall tradeoff

---

## 👤 Author

**Mostafa Shazly** · [LinkedIn](https://linkedin.com/in/mostafa-shazly-148945314) · [GitHub](https://github.com/mostafashazly7)

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1a0a2e,100:0d1117&height=100&section=footer"/>

---

## ⭐ Support
If you like this project, don't forget to ⭐ the repo!
