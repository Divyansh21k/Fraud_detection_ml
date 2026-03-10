# Fraud Detection ML Project

Kaggle Public Score: 0.8495 | [Live Assistant](https://fraud-assistant.onrender.com)

## What is this project?

This is a two phase fraud detection project. In phase 1 I competed on the IEEE-CIS Fraud Detection Kaggle competition to properly understand how fraud detection works at scale. In phase 2 I took what I learned and built a real working assistant that anyone can use.

I did not just run a model and submit. I went through the data properly, built features from scratch, understood what the model was learning using SHAP, and then figured out why the IEEE features could not be used in a real product. That thinking led to phase 2.

## Project Structure
```
Fraud_detection_ml/
├── 01-exploration.ipynb          # EDA, class imbalance, missing values, distributions
├── 02-feature-engineering.ipynb  # 19 engineered features built from raw transaction data
├── 03-baseline-model.ipynb       # First XGBoost model, AUC-PR 0.7025
├── 04-tuning.ipynb               # Hyperparameter tuning, AUC-PR improved to 0.8239
├── 05-interpretability.ipynb     # SHAP analysis, understanding what the model learned
└── 06-kaggle-submission.ipynb    # Final submission, Kaggle public score 0.8495
```

## The Dataset

IEEE-CIS Fraud Detection dataset from Kaggle. 590,540 transactions from Vesta Corporation's real payment platform. 3.5% fraud rate which means the data is heavily imbalanced. Features include transaction details, card information, email domains, device info, and 300+ anonymous Vesta behavioral signals called V columns.

## What each notebook does

01 Exploration
First thing I did was just look at the data. Checked the fraud rate, looked at distributions, found where the missing values were. The V columns had a lot of missingness which told me these are behavioral signals that only fire in certain situations, not always collected. Understanding this before modeling saved me from making bad imputation decisions.

02 Feature Engineering
The raw features alone are not enough. I built 19 new features on top of the raw data. The most useful ones were velocity features, how many times a card showed up, what its average transaction amount was, and what the fraud rate was for a given product type. These gave the model a sense of history across transactions, not just one transaction in isolation.

03 Baseline Model
Ran XGBoost with mostly default parameters to get a number to beat. Got AUC-PR of 0.7025. I used AUC-PR instead of accuracy because with 3.5% fraud rate, a model that predicts everything as legitimate gets 96.5% accuracy which is meaningless.

04 Tuning
Searched over learning rate, tree depth, subsample ratio, and column sample ratio. Also tuned the classification threshold separately because the default 0.5 was wrong for this imbalanced dataset. Tuning the threshold alone gave a meaningful lift. Final AUC-PR was 0.8239 with these params: n_estimators=500, max_depth=8, learning_rate=0.2, subsample=0.9, colsample_bytree=0.8, threshold=0.7015.

05 Interpretability
Used SHAP values to understand what the model was actually learning. Top features were C13, card1_count, and TransactionAmt. The model was picking up on card velocity and unusual amounts relative to card history. This confirmed the model was learning real fraud signals and not just memorizing the training data.

06 Kaggle Submission
Submitted the final predictions. Public leaderboard score came out to 0.8495.

## Results

Baseline AUC-PR was 0.7025. After tuning it went to 0.8239. Kaggle public score was 0.8495.

## Why I built Phase 2

After finishing the competition I wanted to build something real that people could actually use. The problem was the IEEE features are all proprietary Vesta columns. C13, V258, card1 mean nothing to a real person. You cannot ask someone what their C13 value is.

So I retrained on the NeurIPS 2022 Bank Account Fraud Dataset which has fully readable features like housing status, device type, email provider, and credit score. Then I built FraudGuard, a web assistant where you describe a transaction in plain English and get an instant fraud assessment.

[FraudGuard Assistant](https://fraud-assistant.onrender.com) | [Assistant Repo](https://github.com/Divyansh21k/Fraud_assistant)

## How to run
```bash
pip install pandas numpy xgboost scikit-learn shap matplotlib seaborn
jupyter notebook
```

Get the dataset from the IEEE-CIS Fraud Detection competition on Kaggle and place the files in a data folder.

## Tech Stack

Python, XGBoost, SHAP, Pandas, Scikit-learn, Jupyter, Kaggle
