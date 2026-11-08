# Group Number 36 — Human or AI?

This branch contains **Alma's Random Forest work and Jianing's SVM/embedding work**. Abishek has the highest currently recorded public score; his implementation and the combined report still need integration. The final team model/version is not locked.

## Directory layout

```text
submissions/group-number-36/
├── README.md                  Team overview and runnable commands
├── EXPERIMENT_HISTORY.md       Team integration history
├── PULL_REQUEST_DRAFT.md       Draft for the eventual course PR
├── team_results/              Public-score provenance and shared-split verification
├── alma_rf/                   Alma's original RF code, notebooks, evidence and CSV
└── jianing_svm/                Jianing's SVM notebook, optimization, evidence and CSVs
    ├── notebooks/
    ├── src/
    ├── configs/
    ├── data/splits/            IDs only; download raw data separately
    ├── results/
    ├── submissions/
    └── requirements.txt
```

Each member package has its own working directory and environment. Moving Alma's package as a unit preserves its relative paths. Raw data, embeddings, fitted model binaries, caches and virtual environments are excluded. Published notebook outputs are cleared; measured tables, figures, predictions and configurations are retained under each package's results directories.

## Team and current public scores — updated 2026-11-08

| Member | Cornell NetID | Candidate | Local validation AUROC | Kaggle public AUROC |
| --- | --- | --- | ---: | ---: |
| Abishek (Kaggle display name ac3388) | Pending confirmation | `stack_nbsvm_stylo.csv`; implementation not yet included | Not supplied | **0.92847** |
| Liyan (Alma) Yang | ly462 | 21 style features + Random Forest | 0.989913 | 0.89682 |
| Jianing Li | jl4837 | Word/character TF-IDF + chunked MiniLM(0.25), Linear SVM C=1 | 0.974103 | **0.90456** |

Alma's and Abishek's public values are retained from the previous team screenshot record (2026-11-07), not independently rechecked here. Jianing reported 0.90456 on 2026-11-08 for `submission_WordChar_MiniLM025_SVM_C1.csv`. Its bytes match the saved generation receipt; the repository copy is `jianing_svm/submissions/submission_Jianing_WordChar_MiniLM025_SVM_C1.csv`. The score itself is user-reported, not fetched from Kaggle. Public scores are not private/final results. Earlier Jianing score 0.84249 remains in `team_results/public_scores.csv` and historical records. Abishek's official full name and NetID cannot be inferred from a display name.

Abishek remains the leading recorded public candidate, not a confirmed final choice. Preserve all member comparisons, including negative results. The relative ordering on validation differs from the public leaderboard; the cause of this gap is not established.

## Shared validation protocol

Alma and Jianing use **identical article IDs** in the stratified 8,000/2,000 split, seed 42, with 1,000 articles per class in validation. Their manifest formats differ, so membership was compared by ID rather than by raw file hash. See `team_results/split_verification.json`. Abishek's split is not yet verified. AI is positive class 1; ID is never a predictor.

Alma selected representations and RF parameters using training-only three-fold CV. Jianing's bounded representation/C search used the fixed validation set, followed by a training-only three-fold stability check of the selected configuration. The latter is **not nested CV of the search**; the selected validation score may be optimistic. Learned vocabularies and IDFs are refit within calibration/CV training folds. Frozen pretrained MiniLM is disclosed in the recorded configuration; no external prediction API received passages.

## Run Alma's RF package

From `submissions/group-number-36/alma_rf/`, create a separate environment (originally tested with Python 3.12.14):

```bash
python -m venv .venv
# macOS/Linux:
source .venv/bin/activate
# Windows PowerShell instead: .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Put the competition files in `alma_rf/data/`.

```bash
python train.py --train data/train.csv --seed 42 --model-out artifacts/validation_model.joblib --results-dir results/reproduced
python train.py --train data/train.csv --seed 42 --full --model-out artifacts/full_model.joblib --results-dir results/reproduced
python predict.py --test data/test.csv --template data/sample_submission.csv --model artifacts/full_model.joblib --out submission_rf_style.csv
```

Recorded original validation AUROC: 0.989913. During the 2026-11-08 relocation check on Jianing's Python 3.13 environment, the unchanged RF script produced 0.989905 (difference -0.000008). This cross-environment reproduction is recorded separately; the historical result is not overwritten. Candidate CSV: `alma_rf/results/submission_rf_style.csv`. To reproduce exploratory notebooks from the same directory:

```bash
python run_notebook.py Alma_Random_Forest_Exploration.ipynb
python run_notebook.py Alma_RF_Improvements.ipynb
python run_notebook.py Alma_RF_Kaggle_Export.ipynb
```

The export notebook now reads the tracked selected metadata from `results/run_metadata.json`, rather than requiring an absent historical output directory. Alma's feature code and model parameters are unchanged. Details: `alma_rf/EXPERIMENT_HISTORY.md` and `alma_rf/results/EXPERIMENT_REPORT.md`.

## Run Jianing's SVM package

From `submissions/group-number-36/jianing_svm/`, use a separate Python 3.13 environment (recorded experiments used 3.13.15 on Apple Silicon):

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m ipykernel install --prefix "$VIRTUAL_ENV" --name kaggle-jianing --display-name "Python (Kaggle Jianing)"
jupyter lab notebooks/01_Jianing_TF_MiniLM_SVM.ipynb
```

On Windows, activate `.venv\Scripts\Activate.ps1` and use `--prefix "$env:VIRTUAL_ENV"` for kernel installation. Put the supplied competition CSVs in `jianing_svm/data/raw/`. Run all notebook cells once to produce the feature cache and all experiments. First execution downloads the pinned public MiniLM weights; text is processed locally.

Alternatively run without opening the browser:

```bash
# macOS/Linux: permit initial weight download
HF_HUB_OFFLINE=0 python src/run_jianing_notebook.py
# After weights are cached:
python src/run_jianing_notebook.py
```

On Windows, set `$env:HF_HUB_OFFLINE="0"` before the first run. Notebook sections 1–6 cover TF, full-text chunked MiniLM and fusion. Sections 7–8 retain all 30 optimization candidates and the training-only CV diagnostic. Each run saves its configuration, timings, predictions and figures. Large processed matrices remain local.

After the notebook completes:

```bash
# Latest reviewed optimized candidate, full 10,000-row refit:
python src/jianing_optimized_submission.py
# Earlier TF + MiniLM candidate, retained for comparison:
python src/jianing_submission.py
```

The optimized entrypoint reads the recorded selected configuration, reproduces calibrated validation AUROC, and fits on all labels. Sigmoid calibration uses five stratified training-only folds (seed 42), with a fresh vocabulary/IDF within each fold and `ensemble=False`. Output is a new timestamped CSV under `jianing_svm/submissions/`, containing `id,label` where label is P(AI=1). It is not uploaded automatically. The old and optimized prediction scripts share the data/embedding helper in `src/jianing_submission.py`.

### Jianing's recorded progression

| Representation | C | Validation AUROC |
| --- | ---: | ---: |
| Normalized TF | 1 | 0.909056 |
| Full-text chunked MiniLM | 10 | 0.803924 |
| Equal-weight TF + MiniLM | 1 | 0.922313 |
| Character TF-IDF | 1 | 0.972045 |
| Word + character TF-IDF | 1 | 0.972561 |
| Word + character TF-IDF + MiniLM(0.25) | 1 | **0.974103** |

Training-only three-fold means: previous TF+MiniLM 0.916529; optimized candidate 0.967231. These remain in-distribution diagnostics, not a guarantee of Kaggle performance. Complete scores, unsuccessful settings, length-bin comparisons, ROC figures and calibration receipts are under `jianing_svm/results/`. Original receipt filenames retain historical `Jennie` labels; all new member-facing names and executable paths use **Jianing**, with the alias mapping in `results/publication_provenance.json`.

## Report requirements still to finish as a team

| Requirement | Current evidence / remaining work |
| --- | --- |
| Q1: at least three approaches; tuning for at least two | Alma RF and Jianing SVM comparisons integrated; add Abishek's actual implementation/results and final selection rationale. Multiple SVM representations are not three different classifier families. |
| Q2: 500/1,000/2,000/5,000/8,000 learning curve | Not yet included. Keep the same 2,000 validation IDs. |
| Q3: use unlabeled.csv in a controlled experiment | Not yet included; this experiment is required even if the final model does not use unlabeled data. |
| Q4: selected-model gap, at least three bins, five FP and five FN | Member diagnostics exist, but final-team-model analysis must match the eventual selected model and probabilities. |
| Q5: hypothesis-driven improvement | Alma representation experiments and Jianing feature/fusion experiments provide evidence; report controls and limitations. |
| Q6: reproducible code, meaningful history, AI disclosure | Commands and member history included. Final course PR URL, consolidated AI-use statement and complete report remain pending. |

The provided template's 0.90 is a **provisional private-AUROC full-credit threshold**, not a local/public pass criterion; confirm the instructor's final announcement. Do not claim a private score before it is revealed. The final course PR destination recorded by the team is `sekhari/human-or-ai-submissions` (main); pushing this team branch alone is not that final course PR. See `PULL_REQUEST_DRAFT.md`.
