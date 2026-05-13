# Model Performance Log

This file tracks model training runs, hyperparameters, and evaluation results.
Update this file each time you train or tune a model.

---

## Evaluation Metrics Key

| Metric | Description | Target |
|---|---|---|
| **ROC-AUC** | Primary metric — ability to separate high/low risk patients | ≥ 0.70 (stretch: 0.75+) |
| **PR-AUC** | Precision-Recall AUC — better for imbalanced classes | As high as possible |
| **Sensitivity (Recall)** | % of true high-risk patients correctly flagged | Prioritize — missing a patient is costly |
| **Specificity** | % of true low-risk patients correctly cleared | Balance with sensitivity |
| **F1-Score** | Harmonic mean of precision and recall | As high as possible |

> ⚠️ Raw accuracy is NOT used as a primary metric due to the 9% positive class imbalance.

---

## Model Results

### Baseline — Logistic Regression
| Parameter | Value |
|---|---|
| Solver | lbfgs |
| Max iterations | 1000 |
| Class weight | balanced |
| Train date | TBD |

| Metric | Train (CV) | Test |
|---|---|---|
| ROC-AUC | TBD | TBD |
| PR-AUC | TBD | TBD |
| Sensitivity | TBD | TBD |
| Specificity | TBD | TBD |
| F1-Score | TBD | TBD |

---

### Primary — Random Forest
| Parameter | Value |
|---|---|
| n_estimators | TBD |
| max_depth | TBD |
| min_samples_split | TBD |
| class_weight | balanced |
| Train date | TBD |

| Metric | Train (CV) | Test |
|---|---|---|
| ROC-AUC | TBD | TBD |
| PR-AUC | TBD | TBD |
| Sensitivity | TBD | TBD |
| Specificity | TBD | TBD |
| F1-Score | TBD | TBD |

---

### Advanced — XGBoost
| Parameter | Value |
|---|---|
| n_estimators | TBD |
| max_depth | TBD |
| learning_rate | TBD |
| scale_pos_weight | TBD |
| Train date | TBD |

| Metric | Train (CV) | Test |
|---|---|---|
| ROC-AUC | TBD | TBD |
| PR-AUC | TBD | TBD |
| Sensitivity | TBD | TBD |
| Specificity | TBD | TBD |
| F1-Score | TBD | TBD |

---

## Top 10 Features (to be filled after SHAP analysis)

| Rank | Feature | SHAP Importance | Clinical Interpretation |
|---|---|---|---|
| 1 | TBD | TBD | TBD |
| 2 | TBD | TBD | TBD |
| 3 | TBD | TBD | TBD |
| 4 | TBD | TBD | TBD |
| 5 | TBD | TBD | TBD |
| 6 | TBD | TBD | TBD |
| 7 | TBD | TBD | TBD |
| 8 | TBD | TBD | TBD |
| 9 | TBD | TBD | TBD |
| 10 | TBD | TBD | TBD |

---

## Final Model Selection

**Selected model:** TBD
**Justification:** TBD
**Final ROC-AUC on test set:** TBD
