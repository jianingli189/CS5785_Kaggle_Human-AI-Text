# Draft title

[Submission] group-number-36 — ly462, jl4837, [Abishek NetID pending]

## Team

- Kaggle team name: Group Number 36
- Members: Liyan (Alma) Yang (ly462), Jianing (Jennie) Li (jl4837), Abishek (full name / NetID pending)
- Report PR URL field will use: the eventual course PR URL
- Scope: Alma's RF experiments and comparison artifacts only. Abishek's model is currently leading and still being improved; the final version is not locked; Abishek's and Jennie's code and the full report remain to be integrated.

## Summary

Compare six representations with a Random Forest under a fixed stratified 2,000-row validation protocol, using training-only three-fold CV to select the representation and RF settings. Alma's selected RF candidate (not the team final model) achieves validation AUROC 0.989913. Include all comparisons, negative tuning results, diagnostic metrics, and a reproducible 10,000-row refit that regenerates the candidate Kaggle predictions.

The supplied Kaggle screenshot records Public Scores of **0.92847** (Abishek, `stack_nbsvm_stylo.csv`), **0.89682** (Alma, `submission_rf_style.csv`) and **0.84249** (Jennie, `submission_Jennie_TF_MiniLM_SVM_C1.csv`). Abishek is continuing to improve the leading candidate. These are not final/private leaderboard scores. Local validation AUROC is separate.

## Reproducibility (Alma RF only)

- Setup: Python 3.12; python -m pip install -r requirements.txt
- Validation: python train.py --train data/train.csv --seed 42 --model-out artifacts/validation_model.joblib --results-dir results/reproduced
- Full refit: python train.py --train data/train.csv --seed 42 --full --model-out artifacts/full_model.joblib --results-dir results/reproduced
- Submission: python predict.py --test data/test.csv --template data/sample_submission.csv --model artifacts/full_model.joblib --out submission_rf_style.csv
- Seeds: 42 for split, RF, and training CV.
- Verification: the portable scripts reproduced AUROC 0.989913 and all 3,000 probabilities from the original exported candidate.

## Checklist

- [x] Prepared files are only under submissions/group-number-36/
- [x] README documents environment, commands, and seeds
- [x] Raw competition CSVs, secrets, checkpoints, and raw notebook previews excluded
- [x] We did not retrieve hidden labels from RAID or elsewhere
- [ ] Confirm remaining teammate identity and integrate team contributions
- [x] Upload Alma's incremental migration commits preserving the documented local development sequence
- [ ] Integrate Abishek's method, then document the final version and its training/prediction instructions once chosen and Jennie's experiments
- [x] Record current Public Scores and filenames from the supplied Kaggle screenshot
- [ ] Add submission IDs and confirm the final chosen version
- [ ] Open one team PR from a writable course fork into sekhari/human-or-ai-submissions main
