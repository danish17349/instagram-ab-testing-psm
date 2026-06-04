# 📊 Instagram Ads A/B Testing using Propensity Score Matching

This project evaluates the effectiveness of Instagram advertising campaigns using **Propensity Score Matching (PSM)** to estimate the true impact of ad exposure on user conversions.

Instead of directly comparing treatment and control groups—which may contain inherent selection bias—this analysis creates statistically comparable groups using matching techniques before measuring conversion lift.

The project implements and compares two matching approaches:

- Logistic Regression + Caliper Matching
- K-Nearest Neighbors (KNN) Matching

---

## 🎯 The Problem

In real-world advertising experiments, treatment and control users are often not perfectly balanced.

Users who were exposed to an advertisement may differ from those who were not in terms of:

- Age
- Device Type
- Location
- Historical Engagement
- Account Characteristics

A direct comparison of conversion rates can therefore lead to misleading conclusions.

Propensity Score Matching addresses this issue by pairing treated users with statistically similar control users, allowing a more reliable estimation of advertising effectiveness.

---

## 📂 Project Structure

```text
instagram-psm-analysis/
│
├── Dataset/
│   └── Instagram_Ads_Data_Dictionary.xlsx
│
├── Notebooks/
│   ├── 01_Instagram_EDA.ipynb
│   ├── 02_Instagram_PSM_Logistic_Caliper_Matching.ipynb
│   └── 03_Instagram_PSM_KNN_Matching.ipynb
│
└──Instagram_Ads_Data_Dictionary.xlsx
│
└── README.md
```

---

## Dataset
# Instagram Ads A/B Testing — Propensity Score Matching

This project analyzes the effectiveness of Instagram ad campaigns using A/B testing, with a focus on reducing selection bias through **Propensity Score Matching (PSM)**. Rather than comparing treated and control users directly (which can be misleading if the groups aren't similar), we first build a matched dataset where both groups share comparable baseline characteristics — then measure the true conversion lift.

---

## The Problem

In observational A/B tests, the treatment and control groups are often not naturally balanced. Users who saw an ad may differ from those who didn't — in age, device, engagement history, and more. A raw comparison of conversion rates would mix the treatment effect with these pre-existing differences.

PSM solves this by pairing each treated user with a control user who "looks" as similar as possible — so the only meaningful difference left is whether they were treated.

---

## Project Structure

```
instagram-psm-analysis/
│
├── data/
│   └── Instagram_Ads_Data_Dictionary.xlsx   # Column definitions and metadata
│
├── notebooks/
│   ├── 01_Instagram_Day_001_EDA.html                        # Exploratory data analysis
│   ├── 02_Instagram_PSM_Logistic_Caliper_Matching.ipynb     # PSM via logistic regression + caliper
│   └── 03_Instagram_PSM_KNN_Matching.ipynb                  # PSM via K-Nearest Neighbors
│
└── README.md
```

---

## Dataset

The data comes from `workspace.instagram.instagram_extract_data_v_3` — an internal Instagram ads extract containing user-level behavioral and demographic attributes.

Key columns used:

| Column | Description |
|---|---|
| `user_id` | Unique user identifier |
| `treatment_flag` | 1 = saw the ad (test), 0 = did not (control) |
| `conversions` | Whether the user converted after the campaign |
| `age` | User age |
| `gender` | User gender |
| `location_tier` | City tier (metro, tier-1, tier-2, etc.) |
| `device_type` | Mobile, desktop, tablet |
| `account_age_days` | How long the account has existed |
| `is_creator` | Whether the user is a content creator |
| `follower_count_band` | Bucketed follower count |
| `language_pref` | Preferred language |
| `past_engagement_score` | Historical engagement metric |

Full column definitions are in `data/Instagram_Ads_Data_Dictionary.xlsx`.

---

## Notebooks — Run in Order

# 🔍 Notebook 01 — Exploratory Data Analysis (EDA)

This notebook focuses on understanding the structure and quality of the dataset before applying causal inference techniques.

### Analysis Performed

- Missing value analysis
- Outlier detection
- Feature distribution analysis
- Treatment vs Control comparison
- Correlation analysis
- Initial conversion rate exploration

### Objective

Identify data quality issues and understand differences between treatment and control groups before matching.

---

# ⚙️ Notebook 02 — Logistic Regression + Caliper Matching

This notebook implements a rigorous Propensity Score Matching framework.

### Methodology

#### Step 1: Propensity Score Estimation

A Logistic Regression model is trained to estimate the probability of receiving treatment.

**Model Performance**

| Metric | Value |
|----------|----------|
| ROC-AUC Score | 0.9997 |

#### Step 2: Common Support Trimming

Users with extreme propensity scores are removed to ensure overlap between treatment and control groups.

#### Step 3: Caliper Calculation

A caliper threshold is computed using the standard deviation of the logit-transformed propensity scores.

| Metric | Value |
|----------|----------|
| Caliper Multiplier | 0.9 |
| Final Caliper | 0.260599 |

#### Step 4: 1:1 Matching Without Replacement

Each treatment user is matched with the nearest eligible control user within the caliper distance.

### Matching Results

| Metric | Value |
|----------|----------|
| Matched Treatment Users | 814 |
| Matched Control Users | 814 |
| Final Matched Dataset | 1,628 Records |

### Validation

Balance diagnostics are performed using:

- Standardized Mean Difference (SMD)
- Propensity Score Distribution Analysis
- Covariate Balance Assessment

---

# 🤖 Notebook 03 — K-Nearest Neighbors (KNN) Matching

This notebook applies nearest-neighbor matching using propensity scores.

### Methodology

- Estimate propensity scores
- Perform nearest-neighbor matching
- Create matched treatment-control pairs
- Evaluate balance after matching
- Compare conversion outcomes

### Matching Results

| Metric | Value |
|----------|----------|
| Matched Treatment Users | 5,400 |
| Matched Control Users | 5,183 |
| Matched Pair Rows | 5,400 |
| Final Matched Dataset | 10,800 Records |

### Advantages

- Higher sample retention
- Larger matched population
- Increased statistical power

---

# 📈 Matching Comparison

| Metric | Logistic + Caliper | KNN Matching |
|----------|----------|----------|
| Treatment Users Matched | 814 | 5,400 |
| Control Users Matched | 814 | 5,183 |
| Final Dataset Size | 1,628 | 10,800 |
| Match Quality | Higher | Moderate |
| Sample Retention | Lower | Higher |

---

# 💡 Key Findings

- Propensity Score Matching significantly reduced selection bias.
- Logistic Regression achieved strong treatment prediction performance.
- KNN Matching retained a larger portion of the dataset.
- Caliper Matching prioritized match quality over sample size.
- Both methods created more comparable treatment and control groups than the original dataset.

---

# 🛠️ Technologies Used

- Python
- Pandas
- NumPy
- Scikit-Learn
- SciPy
- Matplotlib
- Seaborn
- Jupyter Notebook
- Databricks(Spark Integration)

---

# 📌 Conclusion

This project demonstrates how Propensity Score Matching (PSM) can enhance A/B testing analysis when treatment and control groups are not perfectly randomized.

By applying Logistic Regression + Caliper Matching and KNN Matching, we created statistically comparable user groups before evaluating campaign performance. This helped reduce selection bias and provided a more reliable estimate of the true impact of Instagram ad exposure on user conversions.

The analysis highlights that traditional A/B test results can be misleading when underlying user characteristics differ between groups. Using causal inference techniques such as PSM enables marketers and analysts to isolate the treatment effect more accurately, leading to better data-driven decisions and more trustworthy campaign evaluations.

Overall, this project showcases a practical framework for combining A/B testing principles with causal inference methods to measure advertising effectiveness in observational datasets.