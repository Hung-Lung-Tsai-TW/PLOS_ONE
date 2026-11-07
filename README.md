```markdown
# Code and Data Repository for PLOS ONE Submission

This repository contains the custom computational scripts and meteorological dataset supporting the findings of the manuscript:

> **"Ambient thermal context, hemoglobin, and DXA-derived skeletal status in a clinical cohort: A dual-timescale cross-sectional association study"**

---

## 📁 Repository Contents

- **`r40_20261106_1200_analysis_caption_notes_revised_v2.py`**  
  Custom Python script developed for cohort construction, environmental feature engineering (THI/DTR), statistical association modeling (HC3-robust OLS regressions and date-aligned repeated-record linear mixed models), and classification diagnostics.
- **`29沙鹿.csv`**  
  Ambient meteorological dataset from the local Shalu monitoring station utilized for microclimate exposure assessment.
- **`KEY_VARIABLE_DICTIONARY_R40_REVIEWER_20261106.md`**  
  Key variable dictionary for reviewers, documenting the variables used in the analysis script and their definitions, so that the manuscript tables and figures can be traced to the corresponding code and data fields.
- **`README.md`**  
  Project documentation, data availability policy, software environment requirements, and replication instructions.

---

## 🔒 Data Availability & Ethical Compliance

Raw individual-level electronic health record (EHR) data are restricted and cannot be deposited in a public repository because this study utilized retrospective, de-identified clinical records to protect patient privacy and comply with the Personal Data Protection Act.

Qualified researchers wishing to request access to the de-identified clinical dataset for academic replication may contact the hospital's Institutional Data Access Committee:
- **Entity**: Medical Research Department, Tungs' Taichung MetroHarbor Hospital
- **Contact Email**: [irb@ttmhh.org.tw](mailto:irb@ttmhh.org.tw)  
*(Access is subject to institutional review board approval and data-sharing agreements.)*

All public code and meteorological materials are archived here in full compliance with the **PLOS Data Availability Policy**.

---

## 🛠️ System Requirements & Environment

The analysis pipeline was implemented and verified under **Python 3.10+**. The required core packages include:

- `numpy`
- `pandas`
- `scipy`
- `statsmodels`
- `patsy`
- `scikit-learn`
- `xgboost`
- `lightgbm`

### Dependency Installation
```bash
pip install numpy pandas scipy statsmodels patsy scikit-learn xgboost lightgbm

```

---

## 🚀 Reproduction Instructions

### 1. Data Directory Configuration

Set the environment variable `STUDY_DATA_DIR` pointing to your local directory containing the dataset:

```bash
export STUDY_DATA_DIR="/path/to/data/directory"

```

Ensure that `29沙鹿.csv` (and authorized de-identified clinical files, upon IRB approval) resides within this directory.

### 2. Running the Analysis Pipeline

Execute the Python script sequentially from the terminal:

```bash
python r40_20261106_1200_analysis_caption_notes_revised_v2.py

```

The script will automatically perform:

1. **Cohort Construction**: Applies the absolute pairing window (≤180 days) and clinical validity filters.


2. **Exposure Reconstruction**: Calculates rolling 30-day thermal indices (THI) and diurnal temperature ranges (DTR).


3. **Statistical Modeling**: Estimates primary HC3-robust OLS regressions and date-aligned repeated-record linear mixed-effects models.


4. **Validation & Diagnostics**: Evaluates collinearity diagnostics (VIF), same-record machine-learning benchmarks, and exports structured summary tables.



---

## 📄 License & Attribution

The custom code and open climate dataset in this repository are archived for academic research and scientific reproducibility.

```

```
