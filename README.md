
````markdown
# LQTS Penetrance • APC • MAVE • Events

This repository contains code and data assets to analyze risk of severe cardiac events among **KCNH2 variant heterozygotes** by combining patient-level covariates with variant-level predictors, including multiplexed assays of variant effect (MAVE; trafficking), automated patch-clamp (APC; current density), Bayesian LQTS penetrance estimates, and additional in-silico/structural features. The analysis implements flexible time-to-event modeling to evaluate prognostic value beyond traditional clinical risk factors.

This work accompanies our preprint and subsequent journal article. For context on design and variables referenced below, see the study describing how MAVE, APC, and penetrance features stratify event-free survival in KCNH2 carriers:

- [medRxiv](https://www.medrxiv.org/content/10.1101/2024.02.01.24301443v2)  
- [AHA Journals](https://www.ahajournals.org/doi/10.1161/CIRCULATIONAHA.124.069828?doi=10.1161/CIRCULATIONAHA.124.069828)
---

## Quick start

Clone the repo, open the R project, and knit the analysis R Markdown.

```bash
git clone https://github.com/kroncke-lab/LQTS-Penetrance-APC-MAVE-Events.git
cd LQTS-Penetrance-APC-MAVE-Events
````

In R/RStudio, open `predict_penetrance_kcnh2-v7.Rmd` and knit. The document reads input tables from `data/`, merges patient- and variant-level features, and fits the survival models used in the paper.

If your local filenames differ from what’s listed in the data dictionary below, update the `read_*` calls or file paths near the top of the Rmd accordingly.

---

## Dependencies

* R ≥ 4.2
* Required packages: `tidyverse`, `survival`, `rms` or `flexsurv`, `readr`, `janitor`, `broom`, `ggplot2`

The exact set is declared at the top of `predict_penetrance_kcnh2-v7.Rmd`.

---

## Reproducible workflow

The analysis:

1. Joins patient-level covariates (e.g., sex, QTc, age, event history) to variant-level features (MAVE trafficking score, APC current density, Bayesian penetrance estimate, in-silico/structural annotations).
2. Builds modeling matrices.
3. Fits time-to-event models for severe cardiac events.

This mirrors the study design reported in the manuscript(s) linked above:

* [medRxiv](https://www.medrxiv.org)
* [AHA Journals](https://www.ahajournals.org)

---

## Data dictionary (for files under `data/`)

The tables below document the expected contents of the `data/` directory and the minimal columns the analysis uses when joining and modeling.

If your filenames or column names differ, either rename them to match here or edit the Rmd’s import/rename steps.



## Data dictionary (for files under `data/`)

Unless noted, the preferred join key in this repo is a protein-level variant token. In these files, that key is usually `var` (e.g., `P2A`, `R27S`). If you need HGVS (`p.Pro2Ala`), see the converter snippet at the end of this section.

### Functional assays and features

| File | Purpose | Headers (verbatim) | Notes |
|---|---|---|---|
| `trafficking-whole-enchilada.csv` | Raw/complete trafficking DMS export | `resnum`, `wt`, `res`, `Mut`, `score`, `score_SE`, `type`, `var` | Use `var` as the variant key. `score` is the trafficking metric; `score_SE` is its SE. |
| `trafficking-whole-enchilada-clean.csv` | Cleaned trafficking table | `traff_score`, `score_SE`, `var`, `type` | Same key (`var`); `traff_score` is a normalized version of `score`. |
| `trafficking-whole-enchilada-clean-raw.csv` | Raw pre-clean export (v1) | `score`, `score_SE`, `var`, `type` | Schema mirrors the raw table; pick one canonical source for modeling. |
| `trafficking-whole-enchilada-clean-raw_2.csv` | Raw pre-clean export (v2) | `score`, `score_SE`, `var`, `type` | Redundant with the prior line; keep one. |
| `peak_tail_50mV.csv` | Peak tail current at +50 mV | `var`, `peak_tail` | Values appear normalized; document units/normalization in the manuscript text if relevant. |
| `506variants_CD.csv` | APC current-density summary | `506 variants`, `var`, `peak_tail` | Column name has a space (`506 variants`). Consider renaming to `n_variants` or similar on import. |


### Aggregated predictors and external resources

| File | Purpose | Headers (verbatim) | Notes |
|---|---|---|---|
| `KCNH2[Q12809]_1-1000_VARITY_R_20230207192328620530.csv` | VARITY / meta-predictor bundle for KCNH2 | `p_vid`, `aa_pos`, `aa_ref`, `aa_alt`, `VARITY_R`, `VARITY_R_LOO`, `VARITY_ER`, `VARITY_ER_LOO`, `provean_score`, `sift_score`, `evm_epistatic_score`, `integrated_fitCons_score`, `LRT_score`, `GERP_RS`, `phyloP30way_mammalian`, `phastCons30way_mammalian`, `SiPhy_29way_logOdds`, `blosum100`, `in_domain`, `asa_mean`, `aa_psipred_E`, `aa_psipred_H`, `aa_psipred_C`, `bsa_max`, `h_bond_max`, `salt_bridge_max`, `disulfide_bond_max`, `covelent_bond_max`, `solv_ne_abs_max`, `mw_delta`, `pka_delta`, `pkb


## File layout

* `predict_penetrance_kcnh2-v7.Rmd` — knit-ready analysis document ingesting `data/` and fitting models.
* `data/` — input tables documented above. Raw participant-level data are not included for privacy; analysis expects only de-identified/aggregate/variant-level tables.
* Aux files such as `.Rhistory`, `.RData`, `.gitattributes` are created by R/RStudio.

---

## Outputs

Knit output includes:

* Model summaries
* Hazard ratios
* Discrimination plots
* Partial dependence graphics

These demonstrate the incremental prognostic contribution of MAVE, APC, and penetrance features beyond QTc and sex, consistent with published findings:


## Citing

If you use this code or derived data, please cite the study describing the prognostic value of MAVE/APC features and Bayesian penetrance estimates in **KCNH2-LQTS risk stratification**:

---

## License

Unless otherwise noted in file headers, code is released under a permissive open-source license typical for Kroncke Lab repositories. Adjust this section if a different license applies.

---

## Contact

* Questions? Please open a [GitHub Issue](https://github.com/kroncke-lab/LQTS-Penetrance-APC-MAVE-Events/issues).

* For scientific questions, reference the manuscript and then open an issue with section/table context.

* [medRxiv](https://www.medrxiv.org)

* [AHA Journals](https://www.ahajournals.org)

