# Marketing Data Analytics using Amazon E-commerce Data
### Implications to Retail Business Strategy

> **MKT 4990 – Data Analytics Project**  
> Vaishnavi Perka & Hariharan Jothimani — Data Science Graduates  
> Advisor: Dr. Junhong Min · Michigan Technological University · 2026

[![Status](https://img.shields.io/badge/Status-Completed-brightgreen)](https://github.com/VaishnaviPerka/customer-brand-analytics)
[![Tools](https://img.shields.io/badge/Tools-Python%20%7C%20Pandas%20%7C%20Power%20BI%20%7C%20ANOVA-blue)](https://github.com/VaishnaviPerka/customer-brand-analytics)
[![Data](https://img.shields.io/badge/Dataset-Amazon%20Open%20eCommerce%201.0-orange)](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/YGLYDY)

---

## 📌 Project Overview

This project turns raw Amazon e-commerce transaction data into actionable business insights. By combining over **1 million purchase records** with a **5,027-user demographic survey**, we analyzed customer behavior across product categories, demographics, geographies, and time — then validated findings using statistical hypothesis testing.

*"We combined what people buy with who they are."*

---

## 📦 Data Sources

| Dataset | Size | Contents |
|---|---|---|
| Amazon Transaction Dataset | 1M+ records | Purchase records, product categories, brands, quantities, dates |
| Customer Survey Dataset | 5,027 users | Age, Gender, Income, Family Size, Account Usage, State |

**Key fields used:** Age · Gender · Income · Purchase Quantity · Family Size · Account Usage · State

**Source:** [Open e-Commerce 1.0 · Harvard Dataverse · doi:10.7910/DVN/YGLYDY](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/YGLYDY)

---

## 🧹 Data Cleaning & Preprocessing

Real-world data is messy — about 80% of this project's effort went into cleaning before any analysis began.

| Step | Problem | Solution |
|---|---|---|
| Rename Columns | Cryptic names like `Survey ResponseID`, `ASIN/ISBN` | Standardized to `User ID`, `Product ID`, `State` |
| Fill Missing Values | 89,457 nulls in Category; 89,739 in Title; 87,811 in State | Replaced with `"Unknown"` |
| Fix Date Types | `Order Date` stored as plain text string | Split into separate `Year`, `Month`, `Day` columns |
| Flag Duplicates | 11,624 duplicate rows found | First instance flagged; duplicates removed |

### Category Engineering

400+ raw product codes (e.g., `LAPTOP_ACCESSORIES`, `ABIS_WIRELESS`, `GUITAR_STRINGS`) were mapped via keyword matching into **25 clean, meaningful categories**:

`Electronics` · `Clothing` · `Beauty` · `Food` · `Sports` · `Health` · `Home-Bed-Bath` · `Kitchen` · `Toys` · `Pet Supplies` · `Books` · `Automotive` · `Hardware` · `Baby` · `Jewelry` · `Office Supplies` · `Music & Media` · `Luggage & Travel` · `Garden & Outdoor` · `Industrial` · `Household Essentials` · `Software` · `Collectibles` · `Misc`

---

## 📊 Key Findings

### What Do Customers Buy the Most?
- **Food (18.37%)** and **Home-Bed-Bath (16.84%)** are the top two categories — driven by everyday necessity
- Electronics (12.95%) ranks third
- Essential products dominate demand

### How Purchases Changed Over Time
- Steady growth from **2018 to 2021**
- **Peak in 2020–2021** — pandemic-driven demand surge
- Post-2021 slight decline signals need for re-engagement

### Who Buys More?
- **Young adults aged 25–44** are the most active customers (~62% of purchases combined)
- **Female buyers slightly outnumber males**

### Where Are Customers Located?
Top 5 states by user count:

| State | Share |
|---|---|
| CA | 18.30% |
| FL | 15.10% |
| TX | 14.82% |
| NY | 11.90% |
| PA | 10.51% |

---

## 📐 Statistical Analysis

Three hypothesis tests were run on **1.8M+ merged transaction + survey records** to validate behavioral significance.

### Test 1 — Account Sharing vs. Purchase Quantity (One-Way ANOVA)
> H₀: Number of account users has NO significant effect on purchase quantity

| Metric | Value |
|---|---|
| F-statistic | 5.477 |
| p-value | 0.0009 |
| Result | **Significant (p < 0.05) ✓** |

**Finding:** Solo users (1 account) drive the highest purchases (1,089K). Purchase volume drops sharply as account sharing increases (2 users: 637K → 4+ users: 111K).

---

### Test 2 — Family Size × Age Group vs. Purchase Quantity (Two-Way ANOVA)
> H₀: Family size & age group have NO significant effect on purchase quantity

| Factor | p-value | Significant? |
|---|---|---|
| Family Size | 1.92e-11 | Yes ✓ |
| Age Group | 1.12e-144 | Yes ✓ |
| Interaction (Family × Age) | 2.28e-30 | Yes ✓ |

**Key Findings:**
- Age group has the **strongest independent effect** (F=136)
- **2-person households** make the highest total purchases
- Age moderates how family size influences buying behavior

---

### Test 3 — App Usage × Income vs. Purchase Quantity (Two-Way ANOVA)
> H₀: App usage frequency & income level have NO significant effect on purchase quantity

| Factor | p-value | Significant? |
|---|---|---|
| Account Usage | 2.58e-04 | Yes ✓ |
| Income Level | 1.70e-37 | Yes ✓ |
| Interaction (Usage × Income) | 4.64e-28 | Yes ✓ |

**Key Findings:**
- **Income level** drives the strongest differences in buying volume (F=31.0)
- Heavy app usage alone doesn't predict purchases; Pearson r(usage, qty) ≈ 0.000
- Income moderates how usage affects purchasing behavior

---

## 💼 Business Recommendations

### For Amazon
| Area | Insight | Action |
|---|---|---|
| Target Key Customers | 25–44 age group drives most purchases; females slightly outnumber males | Personalized ads & recommendations for this segment |
| Prioritize Top Categories | Food (18%) and Home-Bed-Bath (16%) dominate | Heavy stocking, promotions, and seasonal campaigns |
| Brand Data Quality | "Unknown" brands account for 4.74% of records | Enforce complete brand tagging in seller onboarding |
| Avoid Wrong Assumptions | Shared accounts buy less; app usage ≠ purchase volume | Rethink family plan strategy; focus on income-aware targeting |

### For Houghton Local Retailers
- **Know your customer:** MTU students & young professionals (25–44) are the prime segment — offer student discounts, loyalty cards, bundle deals
- **Stock what Amazon can't beat:** Locally-sourced products, UP/Michigan-themed goods, niche outdoor gear aligned with MTU's active campus lifestyle
- **Go digital:** Digitize sales data; benchmark pricing using Amazon bestseller data and Keepa API
- **Leverage local loyalty:** Houghton is a tight-knit community — invest in MTU club partnerships, local events, and community sponsorships

---

## 🔮 Future Work

- **ML Prediction Models** — predict customer purchases before they happen
- **Personalized Recommendations** — product suggestions tailored to each customer's demographic profile
- **Real-Time Dashboards** — live data monitoring for instant business decisions
- **Customer Segmentation** — cluster customers into meaningful groups for targeted campaigns

*"We can move from analysis → prediction → personalization."*

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Data Manipulation | Python, Pandas |
| Statistical Testing | One-Way ANOVA, Two-Way ANOVA, Pearson Correlation |
| Visualization | Power BI, Matplotlib / Seaborn |
| Category Engineering | Keyword Matching (Python) |

---

## 📁 Repository Structure

```
customer-brand-analytics/
├── data/
│   └── sample_dataset.csv          # Anonymized sample data
├── analysis/
│   ├── data_cleaning.ipynb              # Preprocessing pipeline
│   ├── merging_datasets.ipynb         # Exploratory analysis & charts
│   └── statistical_tests.ipynb         # ANOVA hypothesis testing        
├── reports/
│   └── CustomerBehaviorAnalysis.pdf     # Full presentation (21 slides)
|   └── customer_behavior.pbix            # Power BI dashboard file
└── README.md
```

---

## 👩‍💻 Authors

**Vaishnavi Perka** — [LinkedIn](https://www.linkedin.com/in/vaishnavi-perka) · [Portfolio](https://vaishnaviperka.github.io)  
**Hariharan Jothimani** — Data Science Graduate, Michigan Technological University
