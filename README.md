# MMCFS: Nested, Leakage-Audited Consensus Feature Selection for Early GDM Prediction

Code accompanying the manuscript:

> K. Harshavardini, R. Dharaniya, R. Mahalakshmi, and V. Kaliraj,
> "Reliability Before Accuracy: Nested, Leakage-Audited Consensus Feature
> Selection for Early Gestational Diabetes Prediction," submitted to
> IEEE Journal of Biomedical and Health Informatics, 2026.

Archived release (DOI): `<ADD ZENODO DOI AFTER RELEASE>`

## What this repository does

1. Combines four metaheuristic feature selectors (PSO, GA, GWO, DFO) through
   accuracy-weighted voting (MMCFS).
2. Evaluates the whole pipeline (imputation, scaling, selection, weighting,
   classifier fitting) inside outer 5-fold stratified cross-validation.
3. Runs the secondary analyses reported in the paper: full-14 vs MMCFS-10,
   repeated 10x5 CV, feature-count curve, optimizer-diversity and
   consensus ablations, missingness checks, clinical baselines, decision
   curves, operating point, TabPFN, SHAP, and a small quantum-kernel pilot.

The paper separates primary from secondary evidence (its Table II). This
README uses the same labels so readers know which results are leakage-free.

## Repository layout

```
README.md
LICENSE
requirements.txt        # pinned versions (see below)
data/                   # empty; dataset is NOT redistributed (see Data)
notebooks/
  c.ipynb               # selectors, MMCFS consensus, nested CV (conference stage)
  J.ipynb               # journal analyses (stability, ablations, DCA, SHAP, ...)
results/                # exported tables and figures (optional)
```

Adjust this block to match what you actually upload.

## Data

- Public Thanjavur GDM cohort, Kaggle user `sumathisanthosh`
  (3,525 rows, 17 columns). Original publication: Sumathi and Meganathan,
  *Comput. Syst. Sci. Eng.*, vol. 40, no. 1, 2022.
- The dataset is not included here. The notebooks download it with
  `kagglehub`, which needs a Kaggle account (accept the dataset terms and
  set up your Kaggle API credentials).
- The file ships as an Excel workbook (`Gestational Diabetic Dat Set.xlsx`),
  not CSV.
- Preprocessing applied before modelling: drop the record identifier and the
  OGTT column; keep the 14 remaining predictors.

## Environment

- Python version: `<ADD>`
- Tested on: `<Google Colab / local OS>`
- Install:

```bash
pip install -r requirements.txt
```

Generate `requirements.txt` from the environment that produced the paper's
numbers (`pip freeze`), and make sure these are pinned: `scikit-learn`,
`mealpy`, `xgboost`, `catboost`, `lightgbm`, `tabpfn`, `shap`, `optuna`,
`qiskit` (and its machine-learning/kernel package, if used), `kagglehub`,
`pandas`, `numpy`, `scipy`.

## How to reproduce

1. Run `notebooks/c.ipynb` first. It defines the four selectors and the MMCFS
   consensus and runs the primary nested evaluation.
2. Run `notebooks/J.ipynb`. It reloads the dataset through `kagglehub` and
   runs the journal analyses.
3. Expected runtime: `<ADD>` (the quantum-kernel pilot takes about 14 minutes
   for 100 samples on a noiseless simulator).

### Mapping from paper to code

| Paper item | Notebook / section | Evidence status |
|---|---|---|
| Table III, IV (outer-fold selections) | `<ADD>` | Primary |
| Primary nested performance (Sec. V-B) | `<ADD>` | Primary |
| Table V, VI (14 vs MMCFS-10) | `<ADD>` | Secondary |
| Fig. 2 (feature-count curve) | `<ADD>` | Secondary |
| Table VII (optimizer diversity, consensus rule) | `<ADD>` | Post-selection |
| Table VIII (Prediabetes, missingness) | `<ADD>` | Post-selection |
| Fig. 3, Table IX (decision curves, operating point) | `<ADD>` | Secondary |
| Fig. 4 (SHAP) | `<ADD>` | Descriptive |
| Table X (quantum-kernel pilot) | `<ADD>` | Secondary |

## Reproducibility details

- Outer split: stratified 5-fold. Seed for outer fold `k` is `42 + k`.
- MEALPY optimizers ignore the global NumPy seed. The seed is passed to each
  optimizer's `solve(...)` call.
- Selector settings: 20 candidates, 30 epochs per optimizer; mask threshold 0.5;
  fitness = negative 3-fold CV accuracy of 5-NN.
- Consensus: weights from 5-fold XGBoost accuracy on each subset, score
  rounded to 3 decimals, feature kept if score >= 0.5.
- Primary classifier: scikit-learn random forest, default settings (100 trees).
- Imputation (median) and standardization are fit on the training partition
  only in the primary pipeline.
- Secondary analyses (marked in the paper) fit imputation and scaling on the
  full cohort or use feature lists derived from the full cohort.
- Random-forest settings used in secondary analyses: `<state the final
  n_estimators / max_depth / min_samples_split after you rerun them>`.
- Small differences from the paper's decimals can occur across library
  versions and hardware.

## Known limitations

- Single public cohort; no external validation.
- Timing of each predictor relative to the OGTT is undocumented in the source.
- The quantum-kernel pilot preprocesses before the train/test split and uses
  100 samples and one feature map; it is a secondary check only.

## Citation

```
@article{harshavardini2026mmcfs,
  title   = {Reliability Before Accuracy: Nested, Leakage-Audited Consensus
             Feature Selection for Early Gestational Diabetes Prediction},
  author  = {Harshavardini, K. and Dharaniya, R. and Mahalakshmi, R. and Kaliraj, V.},
  journal = {IEEE Journal of Biomedical and Health Informatics},
  year    = {2026},
  note    = {Under review}
}
```

## License

`<Choose one, e.g. MIT>`. The dataset has its own terms on Kaggle; check them
before redistributing.

## Contact

K. Harshavardini, harshavardini2003@gmail.com
