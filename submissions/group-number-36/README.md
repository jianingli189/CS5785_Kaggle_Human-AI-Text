# Group Number 36 — Human or AI?

## Contribution status and team model choice

**This directory contains Alma's Random Forest experiments and comparison results, not a finalized team submission. Abishek's model is the current leading candidate and is still being improved.**

All three teammates' methods should be retained for comparison and reproducibility. Currently only Alma's code and results are present; Abishek's and Jennie's methods and the combined team report still need to be integrated. The current `train.py`, `predict.py`, notebooks and `results/submission_rf_style.csv` reproduce **Alma's RF candidate only**.

## Current Kaggle Public Scores (2026-11-07)

Transcribed from the team's Kaggle screenshot supplied by Alma. These are Public Scores, not private/final leaderboard results or local validation scores.

| Contributor | Kaggle submitter | Submission file | Public Score |
| --- | --- | --- | ---: |
| Abishek (called Ali in conversation) | ac3388 | stack_nbsvm_stylo.csv | 0.92847 |
| Liyan (Alma) Yang | Liyan Yang | submission_rf_style.csv | 0.89682 |
| Jianing (Jennie) Li | Jianing Li | submission_Jennie_TF_MiniLM_SVM_C1.csv | 0.84249 |

Abishek's submission is currently the leading candidate and the team's intended direction. He is still improving it; the final model/version is not yet locked. The screenshot does not show a final submission selected. Model internals cannot be established from filenames alone.

The project planning document names the third teammate **Abishek**. His Kaggle display name is **ac3388**; full official name and Cornell NetID remain unconfirmed (the display name alone does not verify a NetID).

## Team

- Kaggle team name: **Group Number 36** (confirmed by the user).
- Liyan (Alma) Yang — **ly462** — Random Forest experiments and candidate predictions.
- Jianing (Jennie) Li — **jl4837** — listed teammate; other method code is not included here.
- Abishek — full name and NetID **pending confirmation**; other method code is not included here.

## Setup

Tested with Python **3.12.14**. From this directory:

```bash
python -m venv .venv
# Windows PowerShell:
.venv\Scripts\Activate.ps1
# macOS/Linux instead: source .venv/bin/activate
python -m pip install -r requirements.txt
```

Download competition data from Kaggle into `data/`: train.csv, test.csv, unlabeled.csv, sample_submission.csv, and optionally train_starter_2000.csv. Raw data and model checkpoints are not included.

## Reproduce Alma's RF validation

```bash
python train.py --train data/train.csv --seed 42 --model-out artifacts/validation_model.joblib --results-dir results/reproduced
```

Expected validation AUROC: **0.989913** with the documented versions, seed and original CSV order. The split is stratified: 8,000 training rows and 2,000 validation rows (1,000 per class).
The 21-feature style transformer is in rf_features.py. The RF uses 200 trees, min_samples_leaf=2, max_features=sqrt, unlimited specified depth, and seed 42.

## Regenerate Alma's RF candidate CSV (not the team final submission)

```bash
python train.py --train data/train.csv --seed 42 --full --model-out artifacts/full_model.joblib --results-dir results/reproduced
python predict.py --test data/test.csv --template data/sample_submission.csv --model artifacts/full_model.joblib --out submission_rf_style.csv
```

The full model is trained on all 10,000 supplied labeled examples. Output columns are id,label, where label is P(AI=1), for 3,000 test IDs in template order. The checked candidate is in results/submission_rf_style.csv. The supplied Kaggle screenshot shows `submission_rf_style.csv` with Public Score **0.89682**. This is separate from the local validation AUROC **0.989913**; matching the filename does not independently verify uploaded file bytes.

## Reproduce experiments / notebooks

```bash
python run_notebook.py Alma_Random_Forest_Exploration.ipynb
python run_notebook.py Alma_RF_Improvements.ipynb
python run_notebook.py Alma_RF_Kaggle_Export.ipynb
```

Run from this team directory. The notebooks also open in PyCharm/Jupyter; install ipykernel if needed for interactive execution.
The improvement notebook reuses the original baseline split/predictions stored in outputs/rf_20261107T193931_779056Z. New runs create separate output folders.
Published notebook outputs are cleared to exclude raw passage previews and machine-specific runtime information; aggregate results are in results/.

## Experimental protocol and results

Representations were selected by 3-fold stratified CV within the 8,000 training examples. Vectorizers were fitted separately within each fold. Four RF settings were compared for the selected representation. Validation IDs were fixed across all comparisons.

| Representation | Mean CV AUROC | Validation AUROC |
| --- | ---: | ---: |
| Word counts | 0.936462 | 0.944298 |
| Word TF-IDF | 0.927524 | 0.932584 |
| Style statistics | 0.985673 | 0.989913 |
| Counts + style | 0.978003 | 0.981946 |
| Binary word presence | 0.922607 | 0.931258 |
| Character TF-IDF | 0.960135 | 0.961611 |

Original RF settings won the four-setting comparison. The main observed gain came from representation changes. Short-text AUROC rose from 0.883867 to 0.982366.
At threshold 0.5, false positives increased from 39 to 49, while false negatives decreased from 206 to 47.
Validation diagnostics informed the exploration, so the result is exploratory. It does not guarantee Kaggle generalization. English-word regexes and heuristic sentence splitting limit multilingual handling.
Unlabeled-data experiments, learning curves, teammate methods and a complete team report are not included.

## History

EXPERIMENT_HISTORY.md records the actual sequence, hypotheses, negative results and verification. LOCAL_COMMIT_HISTORY.txt preserves the original local commit references and messages; the upload is a migration of that existing work.
No hidden test labels or external supervised labels were retrieved. No external LLM API labeled or predicted test passages.
