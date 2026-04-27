# Customer Behavior & Brand Analytics

> Analyzing Amazon customer transaction data to uncover purchasing patterns, brand behavior, and demographic signals — with statistical validation and interactive Power BI dashboards.

[![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)](https://github.com/VaishnaviPerka/customer-brand-analytics)
[![Python](https://img.shields.io/badge/Python-3.x-blue)](https://www.python.org/)
[![Tools](https://img.shields.io/badge/Tools-Power%20BI%20%7C%20pandas%20%7C%20statsmodels-blue)](https://github.com/VaishnaviPerka/customer-brand-analytics)

---

## 📌 Project Overview

This project analyzes a large-scale Amazon purchase dataset (1.85M+ transactions across 5,027 users) linked with survey-collected demographic and lifestyle data. The goal is to surface actionable insights for targeted marketing strategies by identifying behavioral patterns, loyalty signals, and demographic drivers of purchasing behavior.

The dataset is sourced from the [Harvard Dataverse](https://dataverse.harvard.edu/) and includes both purchase history and Qualtrics survey responses collected from real Amazon users.

---

## 🎯 Objectives

- Clean and engineer features from raw Amazon transaction and survey data
- Extract brand names from unstructured product titles using rule-based NLP
- Consolidate 1,800+ Amazon product codes into 26 human-readable categories
- Run statistical tests (ANOVA) to validate behavioral hypotheses across demographic groups
- Merge multi-source datasets for unified analysis
- Build interactive Power BI dashboards to communicate insights to stakeholders
- Develop a brand recommendation framework using historical purchase behavior *(in progress)*

---

## 📁 Repository Structure

```
customer-brand-analytics/
├── notebooks/
│   ├── notebook1_purchases_cleaning.ipynb   # Purchases dataset: cleaning + feature engineering
│   ├── notebook2_survey_cleaning.ipynb      # Survey dataset: cleaning + EDA
│   └── notebook3_merging_analysis.ipynb     # Dataset merging + statistical analysis
├── data/
│   └── sample_dataset.csv
├── reports/
│   ├── Dashboard.pdf  
│   └── Presentation_Slides.pdf              
└── README.md
```

---

## 🔬 Notebook Breakdown

### 📦 Notebook 1 — Purchases Dataset: Cleaning & Category Engineering

**Input:** `amazon-purchases.csv` (1,850,699 rows × 8 columns, Harvard Dataverse)  
**Output:** `cleaned_purchases_dataset.csv`

**Key steps:**

- **Column standardization** — renamed verbose Qualtrics-style column names (e.g., `ASIN/ISBN (Product Code)` → `Product_ID`, `Survey ResponseID` → `User_ID`)
- **Missing value imputation** — ~89K NaNs in `Category` and `Title`, ~88K in `State`, filled with `"Unknown"` to retain valid transaction rows
- **Date extraction** — parsed `Order Date` into separate `Year`, `Month`, and `Day` columns for time-series grouping
- **Duplicate detection & removal** — flagged 11,624 duplicate rows (same user, product, date, price, quantity); flagged before removal for transparency. Final shape: **~1,839,075 rows**
- **Brand name extraction** — rule-based `extract_brand()` function extracting brand from unstructured Amazon product titles using a 12-step priority heuristic (e.g., `"by BRAND"` pattern, quantity prefix stripping, multi-word brand matching, fallback first-token logic), supported by curated word filter lists (`GENERIC_WORDS`, `MARKETING_WORDS`, `COLORS`, etc.)
- **Category engineering** — consolidated 1,872 raw Amazon product codes into 26 broad, human-readable categories (e.g., Electronics, Beauty, Home & Kitchen)
- **Exploratory visualizations** — bar charts, distribution plots

---

### 📋 Notebook 2 — Survey Dataset: Cleaning

**Input:** `survey.csv` (5,027 rows × 23 columns, Harvard Dataverse)  
**Output:** `cleaned_survey_dataset.csv`

**Dataset description:** Demographics and lifestyle survey collected via Qualtrics from Amazon users. Columns cover age, race, gender, income, education, household size, account usage frequency, substance use, disability status, life events, and data privacy attitudes.

**Key steps:**

- **Column renaming** — all 23 Qualtrics question codes renamed to human-readable labels (e.g., `Q-demos-age` → `Age`, `Q-sell-YOUR-data` → `Sell_Data`)
- **Missing value handling** — only `Life_Changes` had NaNs (~3,384) as it was an optional free-text field; filled with `"N/A"`
- **Duplicate check** — no duplicates found (each respondent completed the survey exactly once); 5,027 unique `User_ID`s confirmed
- **Exploratory visualizations** — age distribution (bar chart), gender distribution (pie chart), Amazon usage frequency (count plot), Income vs Education heatmap

---

### 🔗 Notebook 3 — Dataset Merging & Statistical Analysis

**Inputs:** `cleaned_purchases_final.csv`, `cleaned_survey_dataset.csv`  
**Output:** `Merged_Cleaned_final_dataset.csv` (~1,839,075 rows × 34 columns)

**Merging approach:** Inner join on `User_ID`. Result includes 5,026 matched users (1 survey respondent opted out of sharing purchase data — expected by design).

**Post-merge cleaning:**
- Resolved duplicate `State` columns (`State_x` = shipping state from purchases; `State_y` = home state from survey) by standardizing `State_y` full names to 2-letter abbreviations using a state mapping dictionary, then renaming to `Order_State` and `State` respectively

**Statistical Analysis (3 tests):**

| # | Research Question | Method | Variables |
|---|---|---|---|
| 1 | Does account sharing affect purchase quantity? | One-Way ANOVA | IV: `No. of Users`; DV: `Quantity` |
| 2 | Does family size affect purchase quantity, and does income moderate this? | Two-Way ANOVA with interaction | IV1: `Family Size`; IV2: `Income`; DV: `Quantity` |
| 3 | Does Amazon account usage frequency affect purchase quantity? | Two-Way ANOVA + Correlation | IV1: `Acc Usage`; IV2: `Age`; DV: `Quantity` |

Each test follows a formal hypothesis structure (H₀ / H₁) with p-value interpretation at α = 0.05.

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Data Cleaning & Transformation | Python, pandas, numpy |
| Feature Engineering | Rule-based NLP (regex, heuristics) |
| Statistical Analysis | scipy (`f_oneway`), statsmodels (`ols`, `anova_lm`) |
| Visualization | matplotlib, seaborn |
| Dashboarding | Power BI, DAX |
| Data Source | Harvard Dataverse (Amazon Purchases + Qualtrics Survey) |

---

## 📊 Dashboards

Interactive Power BI dashboards covering:

- Customer repurchase frequency and trends
- Brand performance comparison
- Loyalty segmentation (new, returning, churned)
- Cross-brand transition flow


---

## 📈 Key Findings *(so far)*

- Identified 11,624 duplicate transactions (primarily gift cards and recurring purchases) in the raw dataset, flagged and removed for clean analysis
- Brand extraction successfully resolved brand identity for >99.9% of 1.85M product rows using a rule-based NLP pipeline with no ML dependency
- 1,872 raw Amazon product codes consolidated into 26 actionable categories for downstream segmentation
- Demographic variables (family size, income, account usage frequency) tested for relationship with purchase quantity via ANOVA — findings documented in Notebook 3
- Repurchase and behavioral patterns visible across 5,026 matched users with both transaction and demographic data

---

## 🔮 Next Steps

- Complete brand switching matrix and repurchase rate calculations segmented by cohort
- Complete brand recommendation framework using purchase history
- Incorporate customer-level demographic features into loyalty models
- Publish final Power BI report publicly

---

## ⚙️ Setup & Usage

1. Download the raw datasets from [Harvard Dataverse](https://dataverse.harvard.edu/)
2. Clone this repository
3. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy statsmodels
   ```
4. Update the `file_path` variables in each notebook to point to your local data directory
5. Run notebooks in order: **Notebook 1 → Notebook 2 → Notebook 3**

---

## 👩‍💻 Author

**Vaishnavi Perka** — [LinkedIn](https://www.linkedin.com/in/vaishnavi-perka) · [Portfolio](https://vaishnaviperka.github.io)
