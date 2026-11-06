### Code and Data Repository for PLOS ONE Submission

This release contains the exact version of the custom program scripts and dataset used for the manuscript titled "Ambient thermal context, hemoglobin, and DXA-derived skeletal status in a clinical cohort: A dual-timescale cross-sectional association study".

#### Contents:
- **r40_20261106_1200_analysis_caption_notes_revised_v2.py**: Custom python code developed for cohort construction, environmental feature engineering (THI/DTR), statistical association modeling, and classification diagnostics.
- **29沙鹿.csv**: The climate dataset utilized in this study.
- **README.md**: Documentation describing the data structure and instructions on how to run the code.

#### Data Availability Note:
Raw individual-level electronic health record (EHR) data are restricted and cannot be deposited in a public repository because the study used retrospective, de-identified clinical records to protect patient privacy. Qualified researchers may request access from the hospital's Institutional Data Access Committee through the Medical Research Department (email: irb@ttmhh.org.tw), subject to the required ethics approvals.

#### Instructions:
1. **Requirements**: Python 3.10+ with `numpy`, `pandas`, `scipy`, `statsmodels`, `patsy`, `scikit-learn`, `xgboost`, and `lightgbm`.
2. **Data Setup**: Ensure `29沙鹿.csv` is located in the configured data directory (`STUDY_DATA_DIR`).
3. **Run**: Execute `r40_20261106_1200_analysis_caption_notes_revised_v2.py` sequentially to perform feature construction, regression analysis, and generate summary tables.

All materials are archived here to ensure the long-term reproducibility of our findings as required by the PLOS Data Availability Policy.
