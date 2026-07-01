<div align="center">

# 📉 Customer Retention & Churn Analysis — Telco

### Turning raw customer data into actionable retention strategy

[![Python](https://img.shields.io/badge/Python-Pandas%20%7C%20NumPy-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Tableau](https://img.shields.io/badge/Tableau-Dashboard-E97627?style=flat-square&logo=tableau&logoColor=white)](https://www.tableau.com/)
[![Status](https://img.shields.io/badge/Status-Complete-2ea44f?style=flat-square)]()
[![License](https://img.shields.io/badge/License-Educational%20Use-lightgrey?style=flat-square)]()

*An end-to-end churn analysis project — cleaning raw telecom customer data in Python, building an interactive Tableau dashboard, and summarizing findings in an executive report to identify churn drivers and quantify revenue at risk.*

</div>

---

<p align="center">
  <img src="Dashboard_2.png" alt="Dashboard Preview" width="900">
</p>

---

## 📖 Overview

Customer churn is one of the most costly problems for subscription-based businesses. This project analyzes a telecom company's customer base (7,043 customers) to answer three questions:

1. **Who is churning?** — which customer segments have the highest churn rates
2. **Why are they churning?** — which contract terms, payment methods, and service types drive attrition
3. **How much revenue is at risk?** — and which segment should be prioritized first

## 🎯 Key Findings

<div align="center">

| 👥 Total Customers | 📉 Churn Rate | 💵 Avg. Monthly Charges | ⏳ Avg. Tenure | ⚠️ Revenue at Risk |
|:---:|:---:|:---:|:---:|:---:|
| **7,043** | **26.54%** | **$64.76** | **32.37 mo** | **$139,131** |

</div>

> 🔍 **The Big Picture:** Roughly 1 in 4 customers churn, and over half of the associated revenue risk is concentrated in a single, identifiable segment.

- 📑 **Contract type is the strongest churn lever.** Month-to-month customers churn at a far higher rate than customers on one- or two-year contracts.
- 💳 **Payment method matters.** Electronic check users churn at nearly 45%, more than double the rate of customers on automatic bank transfer or credit card payments.
- ⏱️ **Churn is front-loaded.** 47.44% of churned customers leave within their first 12 months, and 76%+ of all churn happens within the first two years — pointing to an onboarding/early-engagement problem.
- 🌐 **Fiber optic customers churn more than double the rate of DSL customers** (40%+ vs. under 20%), suggesting pricing or service quality issues.
- 🚨 **Highest-risk segment:** Month-to-month contract + Electronic check payment — **1,850 customers, 53.7% churn rate**, and **$77,316** in at-risk revenue (**55.6%** of the total revenue at risk).

---

## 🗂️ Repository Contents

| File | Description |
|---|---|
| 📄 `WA_Fn-UseC_-Telco-Customer-Churn.csv` | Original raw dataset (IBM Telco Customer Churn dataset), 7,043 rows × 21 columns |
| 📓 `Task02.ipynb` | Python (Pandas) notebook used to clean and prepare the data |
| ✅ `telco_churn_cleaned.csv` | Cleaned dataset used to build the dashboard, 7,043 rows × 23 columns |
| 📊 `Task02.twb` | Tableau workbook containing the dashboard and all worksheets |
| 🖼️ `Dashboard_2.png` | Full dashboard export |
| 🌐 `churn_by_internet_service_types.png` | Worksheet: churn rate by internet service type |
| 👨‍👩‍👧 `churn_by_partner_and_dependent.png` | Worksheet: churn rate by partner/dependent status |
| 💳 `churn_by_payments.png` | Worksheet: churn rate by payment method |
| ⏳ `churn_rate_by_tenure_bucket.png` | Worksheet: churn rate by tenure bucket |
| 🧮 `contract_X_payment.png` | Worksheet: contract type × payment method risk matrix |
| 📝 `Executive_Report.pdf` | Formal write-up summarizing findings and strategic recommendations |

---

## 🧹 Data Cleaning Process

The raw dataset was cleaned using Python (Pandas) in `Task02.ipynb`. Steps performed:

1. **Loaded** the raw CSV (7,043 rows × 21 columns).
2. **Fixed `TotalCharges`**: stripped whitespace and converted to numeric, coercing invalid entries to `NaN` (11 blank values found), then filled these with `0.0` (these corresponded to customers with 0 tenure).
3. **Recoded `SeniorCitizen`** from binary (0/1) to readable labels (`No`/`Yes`).
4. **Created `ChurnFlag`**, a numeric 0/1 version of the `Churn` column for aggregation and modeling.
5. **Standardized text formatting** (`.str.title()`) across categorical Yes/No columns: `Partner`, `Dependents`, `PhoneService`, `PaperlessBilling`, `Churn`, `SeniorCitizen`.
6. **Flagged anomalies** with a new `AnomalyFlag` column, marking customers with `tenure == 0` but `TotalCharges > 0` as data inconsistencies.
7. **Exported** the cleaned dataset to `telco_churn_cleaned.csv` (7,043 rows × 23 columns).

```python
import pandas as pd
import numpy as np

df = pd.read_csv("WA_Fn-UseC_-Telco-Customer-Churn.csv")

df["TotalCharges"] = pd.to_numeric(df["TotalCharges"].str.strip(), errors="coerce")
df.loc[df["TotalCharges"].isna(), "TotalCharges"] = 0.0

df["SeniorCitizen"] = df["SeniorCitizen"].map({0: "No", 1: "Yes"})
df["ChurnFlag"] = df["Churn"].map({"Yes": 1, "No": 0})

yes_no_cols = ["Partner", "Dependents", "PhoneService", "PaperlessBilling", "Churn", "SeniorCitizen"]
for col in yes_no_cols:
    df[col] = df[col].str.strip().str.title()

df["AnomalyFlag"] = ((df["tenure"] == 0) & (df["TotalCharges"] > 0)).astype(int)

df.to_csv("telco_churn_cleaned.csv", index=False)
```

---

## 📊 Dashboard

The interactive dashboard was built in **Tableau Public/Desktop** (`Task02.twb`) using the cleaned dataset. It includes:

- **KPI summary cards**: overall churn rate, total customers, average monthly charges, average tenure, and estimated revenue at risk
- **Filters**: contract type, internet service, payment method, senior citizen status, and tenure bucket
- **Churn Rate by Contract Type** — bar chart
- **Monthly Charges vs. Tenure** — revenue-at-risk pie chart
- **Churn by Payment Method** — horizontal bar chart
- **Churn by Partner and Dependents** — grouped bar chart
- **Contract × Payment Method** — heatmap risk matrix highlighting the highest-risk customer segment
- **Churn by Internet Service Type** — bar chart
- **Churn Rate by Tenure Bucket** — packed bubble chart


---

## 💡 Strategic Recommendations

Based on the analysis, the following actions are recommended to reduce churn:

1. 🔁 **Mandatory auto-pay incentives** — offer a discount or fee waiver to migrate Electronic Check users to automatic Credit Card or Bank Transfer payments.
2. 📆 **Contract migration campaigns** — target month-to-month customers in their first 12 months with incentives to switch to 1-year contracts.
3. 🌐 **Fiber optic service quality audit** — investigate onboarding, reliability, and pricing perception driving the elevated churn rate among fiber customers.
4. 🤝 **First-year retention framework** — redesign the post-purchase communication journey for the first 90 days to address the concentration of churn in the 0–12 month tenure bucket.

---

## 🧰 Tools Used

<div align="center">

![Python](https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/-Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/-NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Tableau](https://img.shields.io/badge/-Tableau-E97627?style=for-the-badge&logo=tableau&logoColor=white)
![Jupyter](https://img.shields.io/badge/-Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

</div>

---

## 🔗 Data Source

Dataset originally sourced from the [IBM Telco Customer Churn dataset](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) (via Kaggle).

---

## ✍️ Author

<div align="center">

### 👤 Neethu O S

**Customer Retention & Churn Analysis — Telco Project**

*Data Cleaning • Dashboard Design • Business Reporting*

[![LinkedIn](https://img.shields.io/badge/Connect%20with%20me-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/neethu-o-s)

⭐ *If you found this project useful or interesting, consider giving it a star!*

</div>
