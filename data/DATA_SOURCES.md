# Data Sources

Data files are **not stored in this repository** to avoid committing large files and patient-adjacent data to version control. Follow the instructions below to download them locally.

---

## Primary Dataset — UCI Diabetes 130-US Hospitals (1999–2008)

**Download URL:**
https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008

**Steps:**
1. Visit the URL above
2. Click the **Download** button — a `.zip` file will download
3. Unzip the file — you will find:
   - `diabetic_data.csv` — 101,766 rows × 50 columns (main dataset)
   - `IDs_mapping.csv` — lookup table for categorical codes
4. Place both files in `data/raw/`

**Or load directly in Python (no download needed):**
```python
from ucimlrepo import fetch_ucirepo
import pandas as pd

dataset = fetch_ucirepo(id=296)
df = pd.concat([dataset.data.features, dataset.data.targets], axis=1)
```

**Citation:**
> Strack, B., DeShazo, J., Gennings, C., Olmo, J.L., Ventura, S., Cios, K.J., & Clore, J.N. (2014).
> Diabetes 130-US Hospitals for Years 1999-2008 [Dataset].
> UCI Machine Learning Repository. https://doi.org/10.24432/C5230J

**License:** Creative Commons Attribution 4.0 International (CC BY 4.0)

---

## Supplementary Dataset — CMS Texas Medicare Geographic Variation PUF (2014–2023)

**Download URL:**
https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Geographic-Variation

**Purpose:** Used for:
- Benchmarking HealthBridge readmission rates against Texas county and state averages
- Validating FY2024 financial impact estimates in the project outline
- Trend analysis of readmission rates across DFW counties (2014–2023)

Place the downloaded file in `data/raw/` as `cms_tx_geographic_puf_2014_2023.csv`

---

## Processed Data Files

After running `notebooks/01_data_cleaning.ipynb` and `notebooks/03_preprocessing.ipynb`, the following files will be generated in `data/processed/`:

| File | Description | Rows | Source Notebook |
|---|---|---|---|
| `diabetic_clean.csv` | Cleaned raw dataset | ~69,990 | 01_data_cleaning |
| `X_train.csv` | Training features (SMOTE-balanced) | ~100,000+ | 03_preprocessing |
| `y_train.csv` | Training labels (SMOTE-balanced) | ~100,000+ | 03_preprocessing |
| `X_test.csv` | Test features (original imbalance) | ~14,000 | 03_preprocessing |
| `y_test.csv` | Test labels (original imbalance) | ~14,000 | 03_preprocessing |

> ⚠️ These files are excluded from version control via `.gitignore`. Regenerate them by running the notebooks in order.
