# 🏥 Hospital 30-Day Readmission Prediction
### HealthBridge Medical Center — DFW Metroplex
**ADTA 5340 — Discovery and Learning with Big Data | University of North Texas | Spring 2026**

> 📄 **Start here:** [Executive Summary](reports/Executive_Summary.pdf) · [Final Presentation (20 slides)](reports/Readmission_Risk_ML_Presentation.pdf)

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
│   ├── 01_data_cleaning.ipynb                  # Raw data → diabetic_clean.csv
│   ├── 02_eda.ipynb                            # Core exploratory data analysis
│   ├── 02b_eda_extended.ipynb                  # Extended EDA: demographics, medications, SMOTE validation, key findings
│   ├── 03_preprocessing.ipynb                  # Encoding, ICD-9 grouping, split, scaling, oversampling
│   ├── 03a_medication_feature_engineering.ipynb # 23 medication columns → 4 engineered features
│   ├── 04_modeling.ipynb                       # Logistic Regression, Random Forest, XGBoost
│   └── 05_evaluation_shap.ipynb                # Evaluation, SHAP explainability, risk tiers, impact simulation
│
├── 📊 data/
│   └── DATA_SOURCES.md                         # Dataset descriptions and download links (data not tracked)
│
├── 🤖 models/
│   └── MODEL_NOTES.md                          # Performance log, confusion matrices, model configuration
│
├── 📄 reports/
│   ├── Executive_Summary.pdf                   # 2-page project summary
│   ├── Readmission_Risk_ML_Presentation.pdf    # Final 20-slide presentation
│   └── figures/                                # Exported charts
│
├── 📦 assets/
│   └── requirements.txt                        # Python dependencies
│
├── .gitignore                                  # Excludes data files and model artifacts
└── README.md
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
| Medication feature engineering | 23 medication columns condensed into `med_count`, `med_changed_count`, `insulin_active`, `key_med_active` |
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

## 📈 Projected Business Impact (Simulation)

*Scenario estimates for the fictional HealthBridge network — not realized savings.*

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
git clone https://github.com/e2smoothhh/healthbridge-readmission.git
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
03a_medication_feature_engineering.ipynb
04_modeling.ipynb
05_evaluation_shap.ipynb    ← Final results
```

---

## 📊 Key Results — Test Set (13,998 patients, 9% readmitted)

| Model | ROC-AUC | Precision | Recall | F1-Score |
|---|---|---|---|---|
| Logistic Regression | 0.577 | 0.11 | 0.49 | 0.18 |
| Random Forest | 0.568 | 0.14 | 0.03 | 0.05 |
| **XGBoost ★** | **0.591** | **0.13** | **0.37** | **0.19** |

### What we learned
- **Accuracy is misleading on imbalanced data:** Random Forest reached 89.7% accuracy but caught only 2.9% of readmitted patients, so we selected models by ROC-AUC and recall.
- **Top drivers of readmission risk:** length of stay, discharge disposition, admission source and type, lab volume, and number of diagnoses.
- **Modest predictive power (ROC-AUC 0.59):** the dataset lacks richer clinical signals, and XGBoost overfit (train AUC 0.79 vs. test 0.59). The model gives directional insight for care teams but is not deployment-ready.

### Next steps
- Add comorbidity indices (e.g., Charlson), lab trends, prior utilization history, and social determinants of health
- Try cost-sensitive learning and early stopping to reduce overfitting
- Tune the decision threshold around care-team capacity
- Build a Power BI risk-tier dashboard for care managers

---

## 👥 Team

Team project (equal contribution) — ADTA 5340, Dr. Tony Fantasia, University of North Texas, Spring 2026

| Team Member |
|---|
| Chukwuebuka Amobi |
| Chimalis Mushambi |
| **Emmanuel Nwawuihe** (this repository) |
| Bissy Olajide |
| Praise Thomas |

**My contributions:** [ADD — e.g., data cleaning pipeline, ICD-9 grouping, XGBoost modeling, SHAP analysis, presentation sections]

Teammate repository: [chimalis-m/Hospital-Readmission-Risk-Prediction-Model](https://github.com/chimalis-m/Hospital-Readmission-Risk-Prediction-Model)

> Note: `02b_eda_extended.ipynb` and `03a_medication_feature_engineering.ipynb` read the processed `.xlsx` files (`diabetic_clean.xlsx`, `X_train.xlsx`, etc.) produced by notebooks 01 and 03. Data files are excluded from version control.

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
