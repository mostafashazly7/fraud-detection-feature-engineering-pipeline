# 🛡️ IEEE-CIS Fraud Detection: High-Performance Feature Engineering Pipeline

## 📖 Project Overview
This repository contains a professional end-to-end data preprocessing and feature engineering pipeline for the **IEEE-CIS Fraud Detection** challenge. The project focuses on transforming over **2 million transaction and identity records** into a machine-learning-ready format, optimizing memory usage, and preserving data integrity for binary classification.

---

## 🚀 Key Technical Features

### 1. Advanced Data Integration
- **Relational Merging:** Seamlessly joined `train_transaction` and `train_identity` tables using unique Transaction IDs.
- **Memory Management:** Implemented data type downcasting to reduce memory footprint by over 60%, allowing processing on standard hardware.

### 2. Feature Engineering & Transformation
- **Categorical Encoding:** Applied **Label Encoding** and **One-Hot Encoding** to transform high-cardinality categorical features into numerical formats.
- **Numerical Scaling:** Utilized **Min-Max Scaling** to normalize financial data, ensuring features like `TransactionAmt` are on a comparable scale.
- **Handling Sparsity:** Developed strategies for dealing with massive null values inherent in financial datasets.

### 3. Pipeline Scalability
- **Reusable Objects:** Exported all encoders and scalers as `.pkl` files to ensure **Inference Consistency**—applying the exact same transformations to the test set as the training set.

---

## 🛠️ Tech Stack & Environment
* **Operating System:** Ubuntu Linux
* **Language:** Python 3.x
* **Key Libraries:** `Pandas`, `NumPy`, `Scikit-Learn`, `Matplotlib`, `Pickle`
---

## 📁 Project Structure
```text
.
├── Train_Data_Preprocessing.ipynb  # Core transformation logic
├── Test_Data_Preprocessing.ipynb   # Inference pipeline
├── label_encoders_dict.pkl         # Saved categorical mappings
├── min_max_scaler.pkl              # Saved numerical scaler
├── .gitignore                      # Security: Prevents 6GB+ CSV upload
└── README.md                       # Documentation
```
---
## 📥 Dataset Access

* The raw datasets are excluded from this repository due to GitHub's size limits (approx. 6GB total).
* To reproduce the results:

* Download the data from Kaggle IEEE-CIS Fraud Detection.

* Place the .csv files in your local directory.

* The .gitignore file is pre-configured to ensure these large files are never accidentally committed.

---

## 👤 Author: **Mostafa Shazly**
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://linkedin.com/in/mostafa-shazly-148945314)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=flat&logo=github)](https://github.com/mostafashazly7)

---

## ⭐ Support
If you like this project, don't forget to ⭐ the repo!
