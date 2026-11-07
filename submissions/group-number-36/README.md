# Group Number 36 — Human or AI?

## Contribution status and team model choice

**This directory currently contains Alma's Random Forest experiments and comparison results, not the team's final selected model. The team has selected Ali's model for the final Kaggle submission.**

Alma reports a Kaggle score of 89-plus for her RF submission and approximately 92 for Ali's submission. These are approximate, user-reported scores, not independently verified leaderboard measurements; exact scores and submission IDs should be added for the report. They are separate from Alma's local validation AUROC of 0.989913.

All three teammates' methods should be retained for comparison and reproducibility. Currently only Alma's code and results are present; Ali's selected model, Jennie's method, and the combined team report still need to be integrated. The current `train.py`, `predict.py`, notebooks, and `results/submission_rf_style.csv` reproduce **Alma's RF candidate only**. They are not entrypoints or predictions for Ali's model.


## Team

- Kaggle team name: **Group Number 36** (confirmed by the user).
- Liyan (Alma) Yang — **ly462** — Random Forest experiments and candidate predictions.
- Jianing (Jennie) Li — **jl4837** — listed teammate; other method code is not included here.
- Ali — full name and NetID **pending confirmation**; other method code is not included here.

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

The full model is trained on all 10,000 supplied labeled examples. Output columns are id,label, where label is P(AI=1), for 3,000 test IDs in template order. The checked candidate is in results/submission_rf_style.csv. Alma subsequently reported uploading an RF submission with a score of 89-plus; the exact score and mapping to this saved CSV have not been independently verified.

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

## History and AI disclosure

EXPERIMENT_HISTORY.md records the actual sequence, hypotheses, negative results and verification. LOCAL_COMMIT_HISTORY.txt preserves the original local commit references and messages; the upload is a migration of that existing work.
ChatGPT assisted with explanations, feature proposals, coding, execution, debugging, verification, and documentation. Alma reviewed explanations and approved the experimental plan. Automated checks verified sample alignment, split isolation, AUROC recomputation and submission format. User review of the final implementation remains required.
No hidden test labels or external supervised labels were retrieved. No external LLM API labeled or predicted test passages.
