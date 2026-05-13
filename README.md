# 🏥 Hospital 30-Day Readmission Prediction
### HealthBridge Medical Center — DFW Metroplex
**Advanced Data Analytics Capstone Project | AY 2025–2026**

---

## 📋 Project Overview

HealthBridge Medical Center is an 850-bed regional hospital network operating across three campuses in the Dallas–Fort Worth Metroplex. As a Medicare-participating facility, HealthBridge is subject to the **CMS Hospital Readmissions Reduction Program (HRRP)**, which penalizes hospitals with above-average 30-day readmission rates by reducing Medicare reimbursements by up to **3% per applicable discharge**.

This project develops a **supervised machine learning model** trained on historical inpatient data that assigns each patient a readmission risk score at the point of discharge — enabling HealthBridge's care management team to prioritize high-risk patients for targeted post-discharge interventions.

---

## 💼 Business Problem

| Metric | Value |
|---|---|
| HealthBridge 30-Day Readmission Rate | 18.2% (vs. 15.5% national average) |
| Estimated FY2024 HRRP Penalty | ~$2.4M in Medicare reimbursement reductions |
| Total Cost of Preventable Readmissions | ~$6.1M annually |
| Excess Readmission Ratio | 1.17 (17% above expected — triggers penalty tier) |

**HRRP-Qualifying Conditions tracked in this project:**
- Acute Myocardial Infarction (AMI)
- Heart Failure (HF)
- Pneumonia (PN)
- Chronic Obstructive Pulmonary Disease (COPD)
- Hip and Knee Arthroplasty (THA/TKA)
- Coronary Artery Bypass Graft Surgery (CABG)

---

## 🎯 Project Goal

Build a machine learning pipeline that:
1. Predicts whether a patient will be readmitted within 30 days of discharge
2. Assigns a risk score at the point of discharge for clinical staff
3. Reduces HealthBridge's 30-day readmission rate from 18.2% toward the 15.5% national average
4. Demonstrates estimated HRRP penalty savings of $1.8M–$4.2M annually

---

## 📁 Repository Structure

```
healthbridge-readmission/
│
├── 📓 notebooks/
│   ├── 01_data_cleaning.ipynb          # Raw data → diabetic_clean.csv
│   ├── 02_eda.ipynb                    # Exploratory data analysis & visualizations
│   ├── 03_preprocessing.ipynb          # Feature engineering → model-ready files
│   ├── 04_modeling.ipynb               # Logistic Regression, Random Forest, XGBoost
│   └── 05_evaluation_shap.ipynb        # Model evaluation, SHAP feature importance
│
├── 📊 data/
│   ├── raw/                            # Original UCI dataset (not tracked by git)
│   ├── processed/                      # diabetic_clean.csv (not tracked by git)
│   └── DATA_SOURCES.md                 # Dataset descriptions and download links
│
├── 🤖 models/
│   └── MODEL_NOTES.md                  # Saved model artifacts and performance log
│
├── 📄 reports/
│   ├── project_outline.docx            # Full project outline document
│   ├── cleaning_preprocessing_summary/ # Team reference documents
│   └── figures/                        # Charts and visualizations for report
│
├── 📦 assets/
│   └── requirements.txt                # All Python dependencies
│
├── .gitignore                          # Excludes data files and sensitive content
└── README.md                           # This file
```

---

## 📊 Datasets

### Primary — UCI Diabetes 130-US Hospitals (1999–2008)
- **Source:** [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)
- **Size:** 101,766 patient encounters × 50 features
- **License:** CC BY 4.0 — free for academic use, no IRB required
- **Post-cleaning:** 69,990 rows × 48 columns

### Supplementary — CMS Texas Medicare Geographic Variation PUF (2014–2023)
- **Source:** [CMS.gov](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Geographic-Variation)
- **Purpose:** Benchmarking HealthBridge readmission rates against Texas county and state averages, financial impact modeling

> ⚠️ **Data files are not stored in this repository.** Download instructions are in `data/DATA_SOURCES.md`.

---

## 🔬 Methodology

This project follows the **CRISP-DM** (Cross-Industry Standard Process for Data Mining) framework:

```
Business Understanding → Data Understanding → Data Preparation
        → Modeling → Evaluation → Deployment
```

### Phase 1 — Data Cleaning
| Step | Action | Outcome |
|---|---|---|
| Replace ? with NaN | 380,000+ placeholders fixed | Valid missing value format |
| Drop high-missing columns | weight (97%), payer_code (40%) | 48 columns remain |
| Fill partial-missing | race, medical_specialty, diag_1/2/3 → 'Unknown' | All rows preserved |
| Remove death/hospice records | discharge_disposition_id ∈ [11,13,14,19,20,21] | Invalid targets removed |
| Deduplicate patients | Keep first encounter per patient_nbr | Data leakage prevented |
| Fix data types | ID columns int → category | Prevents numeric misinterpretation |

### Phase 2 — Preprocessing
| Step | Action |
|---|---|
| Fix remaining NaN | max_glu_serum, A1Cresult filled with 'None' |
| Binary target variable | readmitted_30: <30 → 1, else → 0 |
| ICD-9 grouping (CCS) | 695+ codes → 9 clinical categories |
| Categorical encoding | Binary, ordinal, one-hot, medication simplification |
| Train/test split | 80/20 stratified — preserves 9% positive class ratio |
| Feature scaling | StandardScaler fitted on training data only |
| Class imbalance | SMOTE applied to training set only (9% → ~50% positive) |

### Phase 3 — Modeling
Three models trained and compared:

| Model | Role | Why |
|---|---|---|
| Logistic Regression | Baseline | Interpretable, establishes minimum AUC benchmark |
| Random Forest | Primary | Handles mixed data, built-in feature importance |
| XGBoost | Advanced | Best-in-class tabular classification, SHAP-compatible |

### Phase 4 — Evaluation
Primary metric: **ROC-AUC** (not raw accuracy — class imbalance makes accuracy misleading)

Additional metrics: Precision-Recall AUC, Sensitivity, Specificity, F1-Score, Confusion Matrix

Target: ROC-AUC ≥ 0.70 (stretch goal: 0.75+)

---

## 📈 Expected Business Impact

| Scenario | Readmission Rate | Estimated Annual Savings |
|---|---|---|
| Current State (Baseline) | 18.2% | — |
| Reduce to National Average | 15.5% | ~$1.8M–$2.4M |
| Achieve Top Quartile | 13.5% | ~$3.5M–$4.2M |

---

## 🛠️ Tech Stack

```
Language:        Python 3.10+
Data:            Pandas, NumPy
Visualization:   Matplotlib, Seaborn, Plotly
ML:              scikit-learn, XGBoost, imbalanced-learn
Explainability:  SHAP
Dashboard:       Power BI / Tableau
Version Control: Git / GitHub
Environment:     Google Colab / Jupyter Notebook
```

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/healthbridge-readmission.git
cd healthbridge-readmission
```

### 2. Install dependencies
```bash
pip install -r assets/requirements.txt
```

### 3. Download the dataset
Follow the instructions in `data/DATA_SOURCES.md` to download the UCI dataset and place it in `data/raw/`.

### 4. Run notebooks in order
```
01_data_cleaning.ipynb      ← Start here
02_eda.ipynb
03_preprocessing.ipynb
04_modeling.ipynb
05_evaluation_shap.ipynb    ← Final results
```

---

## 📊 Key Results *(to be updated as project progresses)*

| Model | ROC-AUC | Precision | Recall | F1-Score |
|---|---|---|---|---|
| Logistic Regression | TBD | TBD | TBD | TBD |
| Random Forest | TBD | TBD | TBD | TBD |
| XGBoost | TBD | TBD | TBD | TBD |

---

## 👥 Team

**Program:** Advanced Data Analytics — Concentration in Artificial Intelligence
**Institution:** *[Your University]*
**Academic Year:** 2025–2026

---

## 📜 License & Citation

Dataset citation:
> Strack, B., DeShazo, J., Gennings, C., et al. (2014). *Diabetes 130-US Hospitals for Years 1999-2008* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5230J

This project is for academic purposes only. HealthBridge Medical Center is a fictional institution created for this capstone project.

---

## 🔗 References

- [CMS Hospital Readmissions Reduction Program](https://www.cms.gov/medicare/payment/prospective-payment-systems/acute-inpatient-pps/hospital-readmissions-reduction-program-hrrp)
- [UCI Diabetes Dataset](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)
- [AHRQ Clinical Classifications Software (CCS)](https://hcup-us.ahrq.gov/toolssoftware/ccs/ccs.jsp)
- [SMOTE — Chawla et al. (2002)](https://arxiv.org/abs/1106.1813)
