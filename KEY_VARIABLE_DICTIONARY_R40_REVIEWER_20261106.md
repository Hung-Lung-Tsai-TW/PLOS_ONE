# Key variable definitions and analysis versions

**R40 revised analysis · Version date: 6 November 2026**

This dictionary summarizes the variables and analysis definitions implemented in the source script. Definitions are fixed for this code release; “fixed” does not imply prospective preregistration. Detailed implementation and audit fields are documented separately with the code.

## 1. Clinical variables and dates

| Variable (code name) | Unit / coding | Operational definition |
|---|---|---|
| Patient identifier (`去個資編號`) | De-identified key | Groups records from the same patient. |
| Pairing identifier (`pairing_row_id`) | Unique row key | Identifies an expanded Hb–DXA pairing row, including a placeholder when no Hb entry is parsed; not a verified examination or specimen ID. |
| DXA date (`骨密度檢查執行日期`) | Calendar date | DXA execution date. |
| Hb date (`Hb_Date`) | Calendar date | Laboratory report date parsed from the source text; not a verified specimen-collection date. Weather is linked to this date. |
| Pairing interval (`Time_Gap_Days`) | Days | Absolute DXA–Hb report-date difference; eligible if ≤180 days. |
| Age (`Age` / analytical `年齡`) | Years | Nonmissing annual-file age on an exact patient–DXA-date match takes priority; otherwise the source-record age is retained. Eligible range: 20–110 years, inclusive. This is source-based age selection, not recalculation from birth date. |
| Recorded sex (`sex_bin`) | Female=0; male=1 | Derived from source labels 女 / 男. Missing or unmapped labels remain unavailable. |
| Hemoglobin (`Hb_Value`) | g/dL | Parsed Hb concentration; values <5 or >22 are masked as missing. |
| Site T-scores (`L_T`, `Left_T`, `Right_T`) | T-score units | Lumbar, left-hip and right-hip fields; values outside −8 to +5 are masked. Hip fields are not further adjudicated as femoral neck or total hip. |
| Minimum available T-score (`overall_T`) | T-score units | Minimum of the available masked site values; missing if all three sites are unavailable. Fixed-site analyses use the named site outcome (S11). |
| Binary classification target (`is_osteoporosis`) | 1 / 0 | 1 if `overall_T≤−2.5`, otherwise 0 among outcome-complete records. Operational category, not an adjudicated diagnosis. |
| Descriptive anemia (`is_anemic`) | 1 / 0 | Male Hb<13 or female Hb<12 g/dL. |
| Descriptive strata | Categories | T-score: ≤−2.5, >−2.5 to <−1, ≥−1. Age: <65, 65 to <80, ≥80; S15 additionally reports <50, 50 to <65, ≥65. |

**Missingness and selection.** Masked values retain their parsed originals and QC flags in controlled working data. Repeated-record eligibility requires evaluable dates, a gap ≤180 days, available Hb/age/sex/minimum T-score, all six core weather fields listed below, and the specified bounds, including mean DTR 0–25 °C. These are study QC rules, not adjudications of the cause of an abnormal result.

Primary analysis retains one eligible row per patient: smallest absolute interval, then earliest DXA date, then the first remaining row under the implemented ordering. Reconstructed-weather sensitivities retain the original representatives. Sequential exclusion counts depend on the stated order (S12); included and excluded records can belong to the same patient.

## 2. Weather variables and construction versions

For temperature T in °C and relative humidity R in %, the index is:

**THI = T − 0.55 × (1 − R/100) × (T − 14.4).**

Window-mean THI is calculated from window-mean T and RH; it is not the average of daily THI. Weather definitions must be read together with the construction version below.

| Variable | Unit | Definition |
|---|---|---|
| `T_mean_30d`; `RH_mean_30d` | °C; % | Window means of daily temperature and relative humidity. |
| `DTR_mean_30d` | °C | Window mean of daily maximum minus minimum valid temperature. |
| `THI_mean_30d`; `THI_mean_180d` | Index units | THI formula applied to the corresponding window-mean T and RH. |
| `THI_std_30d`; `T_std_30d` | Index units; °C | Sample SD of daily THI or daily mean temperature within the window. |
| `THI_Trend_Shock` | Index units | `THI_mean_30d−THI_mean_180d`; a descriptive short-minus-long contrast. |
| `THI_hot_days_365d` | Count | Original rolling-row count of daily THI≥27. |
| `THI_hot_fraction_365d` | Proportion, 0–1 | Reconstructed 365-calendar-day observed hot-day count divided by valid-day count. Missing days are excluded from both numerator and denominator. |

Core cohort weather fields are `T_mean_30d`, `DTR_mean_30d`, `RH_mean_30d`, `THI_mean_30d`, `THI_std_30d` and `THI_Trend_Shock`. Additional classification features require their own complete inputs.

| Analysis version | Window / completeness rule | Reported use |
|---|---|---|
| Original construction | Rolling observed rows: 30 with ≥15 nonmissing values; 180 with ≥90; 365 with ≥180 Boolean entries for the hot count. No minimum hourly-count requirement. | Original main analyses and classification benchmarks. Missing daily THI compares false in the original hot-count expression. |
| Original strict-calendar sensitivity | Complete daily calendar; ≥27 jointly valid T/RH/DTR days in 30 calendar positions, using original daily summaries. No new hourly threshold. | S6. |
| Reconstructed hourly/calendar sensitivity | Separate ≥18/20/24-hour candidates; actual 30/180/365-day windows require ≥27/162/329 valid days and full source-span coverage. Missing observations remain missing. | S13 / S3 Fig, S14 and S16. |

Reconstructed daily T, RH and THI use common valid T/RH hours. DTR uses all valid temperature hours, subject to the same hourly threshold; window summaries then use days with all required daily fields. Hourly screening retains T 0–45 °C and RH 0–100%. Conflicting duplicate hourly values are excluded.

A reconstructed W-day window ending on Hb report date D covers **D−(W−1) through D**. Thus 30 days includes the report day and preceding 29 days. No interpolation is applied. S16 compares the same individuals and folds under old/new weather; replacing hot-day count with hot-day proportion is an explicit definition change.

## 3. Analysis-specific transformations

| Variable / role | Definition and use |
|---|---|
| Calendar adjustment | Fully calendar-adjusted primary and conventional repeated-record models use year indicators and `sin(2πm/12)` / `cos(2πm/12)`, m=1,…,12. Model A (Hb outcome) uses Hb-report dates; Model B (T-score outcome) uses DXA dates. Reference years follow each fitted model (S8). |
| Temporal-order subset | Hb report date on or before DXA, including same-day records. Absolute-gap subsets are 0–30, 31–90 and 91–180 days in either direction (S3). |
| Date proxies | Patient + DXA day or patient + Hb report day; not verified examination/specimen counts. S9 also selects one minimum-gap eligible row per DXA-date proxy. |
| `Hb_between`; `Hb_within` | Patient mean Hb and Hb minus that mean, calculated in the one-per-DXA-date-proxy sample for Mundlak decomposition (S9). |
| `Age_Squared`; `THI_x_Hb`; `THI_x_Age` | Age²; THI30×Hb; THI30×Age, used in S4/S16. Weather interactions are rebuilt for new-weather comparisons. |
| `Severe_Heat_Stress`; `Extreme_Anemia`; `Hb_Deficit` | S4/S16 engineering labels: 1[THI30≥27]; 1[Hb<11]; and signed Hb−13 for males / Hb−12 for females. The Hb<11 feature differs from descriptive anemia. |
| Classification calendar terms | S4/S16 include Hb-month sine/cosine features. S13 compares clinical, clinical+calendar and clinical+calendar+weather sets; its calendar predictors use both date anchors and numeric year trends. |
| Forward-evaluation year | Year of the later DXA/Hb report date. Testing begins two calendar years after the first observed availability year and continues through the last. Training records with overlapping weather windows are excluded (S13 / S3 Fig). |
| Spline knots | Default one-third and two-thirds empirical quantiles of distinct THI values, extracted from the fitted Patsy basis; full precision retained and display rounded separately (S2). |

**Interpretation and denominators.** Patient, pairing-row and date-proxy counts are distinct. Continuous descriptions use nonmissing values and sample SD. Operational T-score categories do not establish diagnostic eligibility in younger adults. Adjustment is limited to the covariates specified in each model.

*Source: `r40_20261106_1200_analysis_caption_notes_revised_v2.py`. This document summarizes existing definitions; no model, cohort, result or table numbering is changed.*
