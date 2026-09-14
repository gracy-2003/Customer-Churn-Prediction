# Customer Churn Prediction — Telco

End-to-end churn analysis on a **7,043-customer telecom dataset**: cleaning, EDA, 8 engineered features, four classifiers benchmarked head-to-head, and a Power BI dashboard on top of the results.

**Stack:** Python (Pandas, NumPy, scikit-learn, XGBoost, Matplotlib) · Power BI

---

## Problem

A telecom provider loses roughly a quarter of its customer base to churn. Knowing the overall rate doesn't help — the business needs to know *which* customers are about to leave, and *what* about them predicts it, early enough to intervene.

## What I built

| Stage | Notebook | What happened |
|---|---|---|
| **Cleaning** | `01_Data_Cleaning.ipynb` | Standardised the 7,043 × 33 raw dataset — type fixes, nulls, constant/ID columns |
| **EDA** | `02_EDA.ipynb` | Churn profiled against contract type, tenure, services and billing |
| **Feature engineering** | `03_Feature_Engineering.ipynb` | Engineered **8 features**: Tenure_Group, Monthly_Charge_Category, CLTV_Category, Total_Services, High_Value_Customer, Customer_Segment, Revenue_Category, Customer_Age_Group |
| **Baseline model** | `04_ML.ipynb` | Logistic regression |
| **Tree models** | `05` – `07` | Decision Tree, Random Forest, XGBoost |
| **Benchmark** | `08_Model_Comparison.ipynb` | All four scored on the same held-out test set |
| **Dashboard** | `Dashboard.pbix` | Power BI view of churn drivers and segments |

**Leakage control.** The raw dataset ships with `Churn_Label`, `Churn_Score`, `Customer_Status`, `Churn_Category` and `Churn_Reason` — all of which encode the answer. An early run scored **100% accuracy and 1.00 ROC AUC**, which is the signature of a leak rather than a good model. Those columns are dropped explicitly in every model notebook, and the honest numbers below are what came out afterwards.

## Results

Held-out test set: **1,409 customers, 374 of them churners (26.5%)**.

| Model | Accuracy | Precision | Recall | F1 | ROC AUC |
|---|---|---|---|---|---|
| Logistic Regression | 0.803 | 0.643 | 0.583 | 0.612 | 0.733 |
| Decision Tree | 0.787 | 0.591 | 0.644 | 0.616 | 0.742 |
| **Random Forest** | **0.909** | **0.887** | 0.754 | **0.815** | **0.860** |
| XGBoost | 0.793 | 0.636 | 0.513 | 0.568 | 0.704 |

## Key findings

- **Random Forest is the strongest model on every metric here** — 0.909 accuracy and 0.860 ROC AUC, against 0.803 / 0.733 for the logistic baseline.
- **Recall is the real constraint.** Even the best model catches ~75% of churners; the logistic baseline catches 58%. In a retention programme, the cost of a missed churner is the one that matters, so recall — not accuracy — is the metric to tune against.
- **Contract type dominates the customer mix**: 3,875 of 7,043 customers are month-to-month, against 1,695 on two-year contracts — the single largest lever on churn exposure in the dataset.
- **Tenure is heavily bimodal**: 2,186 customers are in their first 12 months and 2,239 are past 48 months. The early-tenure block is where retention spend has the most room to work.

## Repository structure

```
Customer-Churn-Prediction/
├── 01_Data_Cleaning.ipynb              # 7,043 × 33 raw → clean
├── 02_EDA.ipynb                        # Churn vs contract, tenure, services, billing
├── 03_Feature_Engineering.ipynb        # 8 derived features
├── 04_ML.ipynb                         # Logistic regression baseline
├── 05_Decision_Tree.ipynb
├── 06_Random_Forest.ipynb
├── 07_XGBoost.ipynb
├── 08_Model_Comparison.ipynb           # Head-to-head benchmark
├── Dashboard.pbix                      # Power BI dashboard
├── Telco_customer_churn.xlsx           # Raw dataset
├── customer_churn_clean.xls            # Post-cleaning
└── customer_churn_feature_engineered.xls
```

## How to run it

1. Run the notebooks in numeric order with Jupyter — they need `pandas`, `numpy`, `scikit-learn`, `xgboost` and `matplotlib`.
2. Notebooks `04`–`07` each read `customer_churn_feature_engineered.csv`, produced by `03`.
3. Open `Dashboard.pbix` in Power BI Desktop and repoint the source to your local copy.

---

**Author:** Gracy Srivastava — Data Analyst (SQL · Power BI · Python)
[LinkedIn](https://www.linkedin.com/in/gracy2003) · [GitHub](https://github.com/gracy-2003)
