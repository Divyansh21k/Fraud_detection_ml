# 🔍 Fraud Detection ML — IEEE-CIS Kaggle Competition

### Kaggle Public Score: 0.8495 | [🤖 Live Assistant](https://fraud-assistant.onrender.com)

## About This Project

This is Phase 1 of a two phase fraud detection project. I competed on the IEEE-CIS Fraud Detection Kaggle competition to properly understand how fraud detection works at scale before building anything production ready.

The dataset has 590,540 real transactions from Vesta Corporation's payment platform. I went through the data properly, built features from scratch, understood what the model was learning using SHAP, and ended up with a Kaggle public score of 0.8495.

After finishing I realized the IEEE features are all proprietary Vesta columns — C13, V258, card1. No real person knows what those mean so I could not build a usable product on top of them. That thinking led to Phase 2.

## Why This Project

I wanted to understand fraud detection properly, not just run a model and submit. Fraud is one of the most interesting ML problems because the data is heavily imbalanced, the features are often engineered rather than raw, and the cost of a false negative is very different from the cost of a false positive. I wanted to experience all of that firsthand.

## 📁 Project Structure
```
Fraud_detection_ml/
├── 01_exploration.ipynb          # EDA, class imbalance, missing values, distributions
├── 02_feature_engineering.ipynb  # 19 engineered features built from raw transaction data
├── 03_baseline_model.ipynb       # First XGBoost model, AUC-PR 0.7025
├── 04_model_tuning.ipynb         # Hyperparameter tuning, AUC-PR improved to 0.8239
├── 05_shap_values.ipynb          # SHAP analysis, understanding what the model learned
└── 06_kaggle_submission.ipynb    # Final submission, Kaggle public score 0.8495
```

## ✨ What Each Notebook Does

### 🔎 01 — Exploration
First look at the data. 590,540 transactions, 3.5% fraud rate, heavy missingness in the V columns. I spent time understanding what each column family actually represents before touching the model. This saved me from making bad decisions later.

### ⚙️ 02 — Feature Engineering
Built 19 new features on top of the raw data. The most useful ones were velocity aggregates — how many times a card appeared, its average transaction amount, and the fraud rate for a given product type. These gave the model memory across transactions, not just one transaction in isolation.

### 🤖 03 — Baseline Model
Ran XGBoost with mostly default parameters to get a number to beat. AUC-PR of 0.7025. I used AUC-PR instead of accuracy because with 3.5% fraud rate a model that predicts everything as legitimate gets 96.5% accuracy which is completely meaningless.

### 🎯 04 — Tuning
Searched over learning rate, tree depth, subsample ratio, and column sample ratio. Also tuned the classification threshold separately because the default 0.5 was wrong for this imbalanced dataset. Tuning the threshold alone gave a meaningful lift. Final AUC-PR was 0.8239.

### 🧠 05 — Interpretability
Used SHAP values to understand what the model was actually learning. Top features were C13, card1_count, and TransactionAmt. The model was picking up on card velocity and unusual amounts relative to card history. This confirmed the model was learning real fraud signals and not just memorizing training data.

### 🏆 06 — Kaggle Submission
Submitted the final predictions. Public leaderboard score came out to 0.8495.

## 📊 Results

| Stage | AUC-PR |
|---|---|
| Baseline XGBoost | 0.7025 |
| After hyperparameter tuning | 0.8239 |
| Kaggle public leaderboard | **0.8495** |

Best parameters: n_estimators=500, max_depth=8, learning_rate=0.2, subsample=0.9, colsample_bytree=0.8, threshold=0.7015

## 🛠️ Tech Stack

**ML & Data**
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-FF6600?style=flat)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![SHAP](https://img.shields.io/badge/SHAP-FF6600?style=flat)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

## 🚀 How to Run
```bash
pip install pandas numpy xgboost scikit-learn shap matplotlib seaborn jupyter
jupyter notebook
```

Get the dataset from the IEEE-CIS Fraud Detection competition on Kaggle and place the files in a data folder.

## 🤖 Phase 2 — FraudGuard Assistant

After this competition I built FraudGuard, a conversational fraud detection assistant. The IEEE features could not be used in plain English conversation so I retrained on the NeurIPS 2022 Bank Account Fraud Dataset which has fully readable features and built a web assistant on top of it.

[Try FraudGuard Live](https://fraud-assistant.onrender.com) | [Assistant Repo](https://github.com/Divyansh21k/Fraud_assistant)

Made with ❤️ by Divyansh Kharnal
