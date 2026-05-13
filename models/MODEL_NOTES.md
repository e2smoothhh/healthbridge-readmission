# Model Performance Log

## Final Results — Test Set (13,998 patients | 9% positive class)

| Model | Accuracy | ROC-AUC | Precision (1) | Recall (1) | F1 (1) |
|---|---|---|---|---|---|
| Logistic Regression | 61.2% | 0.577 | 0.11 | 0.49 | 0.18 |
| Random Forest | 89.7% | 0.568 | 0.14 | 0.03 | 0.05 |
| **XGBoost ★ Selected** | **71.4%** | **0.591** | **0.13** | **0.37** | **0.19** |

## Final Model Selection
**Selected model:** XGBoost
**Justification:** Highest ROC-AUC (0.591) and best balance between identifying
high-risk patients and avoiding false alarms. Random Forest's 89.7% accuracy
was misleading — it correctly flagged only 2.9% of actual high-risk patients.
**Final ROC-AUC on test set:** 0.591

## Top Features — XGBoost
| Rank | Feature | Importance |
|---|---|---|
| 1 | time_in_hospital | 0.40 |
| 2 | discharge_disposition_id | 0.27 |
| 3 | admission_source_id | 0.18 |
| 4 | admission_type_id | 0.16 |
| 5 | num_lab_procedures | 0.10 |
| 6 | num_diagnoses | 0.09 |
| 7 | diabetesMed_enc | 0.08 |
| 8 | max_glu_serum | 0.07 |
| 9 | A1Cresult | 0.05 |

## Confusion Matrices

### XGBoost
| | Predicted 0 | Predicted 1 |
|---|---|---|
| Actual 0 | 9,531 | 3,210 |
| Actual 1 | 793 | 464 |

### Random Forest
| | Predicted 0 | Predicted 1 |
|---|---|---|
| Actual 0 | 12,517 | 224 |
| Actual 1 | 1,221 | 36 |

### Logistic Regression
| | Predicted 0 | Predicted 1 |
|---|---|---|
| Actual 0 | 7,943 | 4,798 |
| Actual 1 | 640 | 617 |

## XGBoost Configuration
- n_estimators: 300
- max_depth: 6
- learning_rate: 0.05
- scale_pos_weight: 10.13
- Overfitting noted: Train AUC 0.79 vs Test AUC 0.59
- Recommended fix: early_stopping_rounds=20
