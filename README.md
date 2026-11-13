# Code and data for the PLOS ONE submission

**Ambient thermal context, hemoglobin, and DXA-derived skeletal status in a clinical cohort: A dual-timescale cross-sectional association study**

This repository provides the analysis code, Shalu meteorological data and key variable dictionary. The pipeline reconstructs the cohorts, calculates the original-specification and revision analyses, and exports the manuscript tables and figures. Individual-level clinical data are restricted and are not included in this repository.

The current entry point is **`r41_analysis_v3.py`**, exported from `R41_analysis_v3.ipynb`. It retains the original and revision analyses and adds **S17 Table**, a matched legacy-versus-reconstructed-weather sensitivity analysis for the original Table 5, S5 Table and S7 Table B/C classification specifications. The previous R40 Python entry point has been replaced; its earlier versions remain available in Git history.

## Supported execution environment

**Run the current script in a Google Colab Python notebook with Google Drive mounted.** Its startup imports `google.colab`, mounts Drive, and sets the study paths. Running the script directly from an ordinary local terminal is not the supported workflow for this version.

The script records the actual Python version (`sys.version`) and imported package versions for each run; it does not enforce an exact Python patch version. Retain the generated `runtime_versions.json` with the results rather than assuming that every Colab session has the same environment.

The following package versions are the exact analysis-library requirements checked by `r41_analysis_v3.py`:

| Package to install | Import name | Required version |
|---|---|---|
| numpy | `numpy` | 2.4.6 |
| pandas | `pandas` | 2.2.3 |
| scipy | `scipy` | 1.16.3 |
| statsmodels | `statsmodels` | 0.15.0 |
| patsy | `patsy` | 1.0.3 |
| matplotlib | `matplotlib` | 3.10.0 |
| scikit-learn | `sklearn` | 1.6.1 |
| xgboost | `xgboost` | 3.4.1 |
| lightgbm | `lightgbm` | 4.6.0 |
| openpyxl | `openpyxl` | 3.1.5 |
| Pillow | `PIL` | 11.3.0 |

Keep Colab's working IPython installation. The script imports and records IPython but explicitly exempts it from exact-version equality. Although its `REQUIRED_VERSIONS` dictionary retains the earlier IPython baseline `9.18.1`, upgrading to `9.18.1` is not required.

## Repository contents

| File | Purpose |
|---|---|
| `r41_analysis_v3.py` | Full Colab analysis and publication-export pipeline, including the appended R41/S17 sensitivity analysis. |
| `29沙鹿.csv` | Frozen Shalu hourly meteorological input used by the analysis. Official hourly-observation dataset: [AQX_P_217](https://data.moenv.gov.tw/en/dataset/detail/AQX_P_217); station metadata: [AQX_P_07](https://data.moenv.gov.tw/en/dataset/detail/AQX_P_07). For reproduction, use the CSV supplied with the cited repository version. |
| `KEY_VARIABLE_DICTIONARY_R41_REVIEWER_20261112.md` | Concise frozen variable definitions and code-field mapping for R41, including the S17 populations, weather definitions, split preservation and evaluation rules. |
| `README.md` | Colab setup, input requirements, output locations and controlled-access information. |

**Versioned release:** [v1.0.5](https://github.com/Hung-Lung-Tsai-TW/PLOS_ONE/releases/tag/v1.0.5) packages the R41 script, R41 variable dictionary, public weather file and this Colab documentation. Earlier tags are retained; v1.0.4 identifies the earlier R40 snapshot.

Use the script and public weather file from the same repository version. Record the Git commit used for each reproduction; `main` can change.

## Required files and Drive layout

The unmodified startup uses this exact directory:

```text
/content/drive/MyDrive/Colab Notebooks/HB-DXA/
```

In Google Drive, this corresponds to **My Drive → Colab Notebooks → HB-DXA**. Place the files directly in the locations shown below, retaining their exact filenames, including the full-width `～` character.

```text
HB-DXA/
├── 骨密度及宏碁骨密清單(去個資化)_20230101～20231231.csv
├── 骨密度及宏碁骨密清單(去個資化)_20240101～20241231.csv
├── 骨密度及宏碁骨密清單(去個資化)_20220101～20221231.csv
├── 骨密度及宏碁骨密清單(長期)(去個資化)_20200101～20201231.csv
├── 骨密度及宏碁骨密清單(去個資化)_20200101～20201231.csv
├── 骨密度及宏碁骨密清單(長期)(去個資化)_20180101～20181231.csv
├── 骨密度及宏碁骨密清單(長期)(去個資化)_20190101～20191231.csv
├── 骨密度及宏碁骨密清單(去個資化)_20210101～20211231.csv
├── 2017.csv
├── 2018.csv
├── 2019.csv
├── 2020.csv
├── 2021.csv
├── 2022.csv
├── 2023.csv
├── 2024.csv
├── 29沙鹿.csv
└── publication_assets/
    └── Fig2_20260921.tif
```

The inputs comprise eight clinical-report CSVs, eight annual DXA/age CSVs, one weather CSV and the original Figure 2 TIFF. The clinical-report and annual files require authorized access. The public weather file alone is insufficient to execute the full clinical pipeline.

An existing merged master CSV, previously fitted models and earlier output folders are not required inputs. The script builds its working master from the raw reports in a fresh private run directory.

**Figure 2 is a required input asset**, retained unchanged rather than regenerated. Obtain the original TIFF from the authors' publication materials and place it under `publication_assets/`. It is not supplied by the Python script. Renaming or re-saving a different image does not satisfy the source check. The expected SHA-256 is:

```text
d7990303b6cebb1dfb6573871a816ed1f1d9c7377d66dab9b2ab34db1cd46da8
```

If a different Drive root is necessary, edit the single `COLAB_STUDY_ROOT` assignment in the startup section of the local script copy. Merely setting `STUDY_DATA_DIR` beforehand will not override it: startup assigns that environment variable again. Keep the input filenames and `publication_assets` subdirectory consistent with the script.

## Run in Colab

### 1. Prepare the notebook and packages

Open a new Google Colab Python notebook. Before importing the analysis libraries, run this cell to install the exact analysis-library versions:

```python
%pip install numpy==2.4.6 pandas==2.2.3 scipy==1.16.3 statsmodels==0.15.0 patsy==1.0.3 matplotlib==3.10.0 scikit-learn==1.6.1 xgboost==3.4.1 lightgbm==4.6.0 openpyxl==3.1.5 Pillow==11.3.0
```

After successful installation, **restart the Colab runtime/session before running the analysis** so previously imported packages are not retained in memory. Do not force an IPython upgrade. If installation fails, or a required version cannot be used in the selected Colab runtime, resolve that environment mismatch before continuing; do not silently substitute newer versions or disable the script's version checks.

### 2. Obtain and identify the code version

In the restarted notebook, run the following once in a fresh runtime:

```python
!git clone https://github.com/Hung-Lung-Tsai-TW/PLOS_ONE.git /content/PLOS_ONE
!git -C /content/PLOS_ONE checkout --detach v1.0.5
!git -C /content/PLOS_ONE rev-parse HEAD
```

Save the printed commit identifier with the run records. To reproduce a particular release or revision, check out that release's tag or commit before executing the script and use its corresponding weather file. If `/content/PLOS_ONE` already exists, use the intended existing checkout rather than treating a failed clone as a successful update.

Copy `29沙鹿.csv` from the selected repository version to the Drive input directory, and prepare the authorized clinical files and original Figure 2 asset as listed above.

### 3. Mount Drive and verify the required figure

Run this short check before starting the computationally intensive analyses. The script will also validate the required CSVs and package versions at startup.

```python
from google.colab import drive
from pathlib import Path
import hashlib

drive.mount('/content/drive')
study_root = Path('/content/drive/MyDrive/Colab Notebooks/HB-DXA')
if not study_root.is_dir():
    raise FileNotFoundError(f'Study directory not found: {study_root}')
fig2 = study_root / 'publication_assets' / 'Fig2_20260921.tif'
if not fig2.is_file():
    raise FileNotFoundError(f'Required original Figure 2 missing: {fig2}')
expected_fig2_sha256 = 'd7990303b6cebb1dfb6573871a816ed1f1d9c7377d66dab9b2ab34db1cd46da8'
if hashlib.sha256(fig2.read_bytes()).hexdigest() != expected_fig2_sha256:
    raise ValueError('Figure 2 differs from the required original TIFF.')
print('Drive path and original Figure 2 verified.')
```

If the study root was changed in the script, make the same path change in this check.

### 4. Execute the full pipeline

Run the script **inside the Colab notebook kernel**:

```python
%run /content/PLOS_ONE/r41_analysis_v3.py
```

The startup mounts Drive again if necessary, assigns the study paths, checks inputs and versions, and creates fresh run directories. Analyses execute in their existing dependency order. Do not run only the final export statements in a fresh session: they depend on the models and summaries calculated earlier in the script.

After a runtime reset or loss of the in-memory analysis objects, rerun the complete script with the required environment and inputs. A failed or interrupted run must not be treated as a complete publication package.

## Output locations and completion

Each complete execution creates a new publication directory:

```text
HB-DXA/OUTPUT/Publication_clean_<UTC timestamp>_<run identifier>/
```

The base export covers **27 publication items**: Tables 1–5, S1–S16 Tables, Figs 1–3 and S1–S3 Figs, without renumbering. It writes:

- Table workbooks and canonical CSVs, such as `Table1.xlsx`, `Table1.csv`, `S16_Table.xlsx` and `S16_Table.csv`.
- CSVs for every workbook sheet under `Sheets/`; a canonical table CSV contains only the selected display panel, not necessarily the entire workbook.
- TIFF figures and available PNG companions, with caption/note text files.
- `Export_log.csv`, `Worksheet_index.csv`, `Path_redaction_log.csv`, `runtime_versions.json`, `Historical_display_differences.csv` and `File_manifest.csv`.

**The message `Completed 27 publication items` marks completion of the base export only.** The R41/S17 analysis runs afterwards. It writes an additional publication item, bringing the full script's scope to **28 items**, in a separate subdirectory of the same publication run:

```text
HB-DXA/OUTPUT/Publication_clean_<UTC timestamp>_<run identifier>/
└── S17_R41_<UTC timestamp>_<run identifier>/
    ├── Supplementary_Tables/
    │   ├── S17_Table_rerun.xlsx
    │   └── S17_Table_for_Word.html
    ├── Notes/
    │   └── S17_Table_caption_and_notes.txt
    ├── Audit/
    ├── run_status.json
    └── S17_manifest.csv
```

S17 retains all three hourly-completeness thresholds (18/20/24 hours) and the original classification specifications. Within each threshold, clinical-only, legacy-weather and reconstructed-calendar-weather arms use the same retained participants and train/test splits. The analysis includes 189 model fits and paired test-record bootstrap summaries for the fixed-test-set Brier comparison. Its notes explicitly identify the change from a 365-day hot-day count to a hot-day fraction among valid observed days. S13 remains the separate forward-evaluation analysis; S16 covers the original S4 six-classifier specification.

Verify that the final `R41／S17 4` summary reports `Completed=True`, `S17_exported=True`, `All_passed=True`, and matching requested/succeeded fit counts. These fields are also recorded in the S17 subdirectory's `run_status.json`. The base `File_manifest.csv` is produced before S17 and does not inventory the appended S17 files; retain **both** manifests. A successful base export followed by an S17 error is not a completed R41 run.

Retain the actual runtime-version record, both file manifests and the code commit identifier. The script calculates current results from inputs; historical manuscript values retained for reconciliation are labeled separately and do not replace those calculations. Older notebook names and hashes inside provenance records identify the source specifications and are not execution entry points.

Private working copies, record-level audits and intermediate analysis outputs are stored separately under:

```text
HB-DXA/.private_runs/<UTC timestamp>_<run identifier>/
```

Do not upload `.private_runs` or the clinical inputs to the public repository. The final publication-export directory is separate from this private working area. Previous run directories are retained.

## Data access

Individual-level EHR data were obtained from Tungs' Taichung MetroHarbor Hospital and are not publicly distributed because of patient confidentiality and potential re-identification. Qualified researchers may request access through the hospital's institutional access process:

- **Institutional contact:** Medical Research Department, Tungs' Taichung MetroHarbor Hospital.
- **Email:** [irb@ttmhh.org.tw](mailto:irb@ttmhh.org.tw).
- **Conditions:** Institutional review, required ethics approvals and data-sharing agreements.

Public access to this repository does not grant access to the restricted clinical inputs. Aggregate results and methodological details are provided in the manuscript and Supporting Information. The public code and variable dictionary document the original and reconstructed weather definitions separately.
