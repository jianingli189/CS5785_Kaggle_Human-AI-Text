# Draft title

[Submission] group-number-36 — ly462, jl4837, [Ali NetID pending]

## Team

- Kaggle team name: Group Number 36
- Members: Liyan (Alma) Yang (ly462), Jianing (Jennie) Li (jl4837), Ali (full name / NetID pending)
- Report PR URL field will use: the eventual course PR URL
- Scope: Alma's RF experiments and comparison artifacts only. Ali's model is the team's selected final model; Ali's and Jennie's code and the full report remain to be integrated.

## Summary

Compare six representations with a Random Forest under a fixed stratified 2,000-row validation protocol, using training-only three-fold CV to select the representation and RF settings. Alma's selected RF candidate (not the team final model) achieves validation AUROC 0.989913. Include all comparisons, negative tuning results, diagnostic metrics, and a reproducible 10,000-row refit that regenerates the candidate Kaggle predictions.

The team selected Ali's model based on user-reported Kaggle scores of approximately 92 for Ali versus 89-plus for Alma. Exact scores and submission IDs remain to be documented. Local validation AUROC is a separate measurement.

## Reproducibility (Alma RF only)

- Setup: Python 3.12; python -m pip install -r requirements.txt
- Validation: python train.py --train data/train.csv --seed 42 --model-out artifacts/validation_model.joblib --results-dir results/reproduced
- Full refit: python train.py --train data/train.csv --seed 42 --full --model-out artifacts/full_model.joblib --results-dir results/reproduced
- Submission: python predict.py --test data/test.csv --template data/sample_submission.csv --model artifacts/full_model.joblib --out submission_rf_style.csv
- Seeds: 42 for split, RF, and training CV.
- Verification: the portable scripts reproduced AUROC 0.989913 and all 3,000 probabilities from the original exported candidate.

## Checklist

- [x] Prepared files are only under submissions/group-number-36/
- [x] README documents environment, commands, seeds, and AI use
- [x] Raw competition CSVs, secrets, checkpoints, and raw notebook previews excluded
- [x] We did not retrieve hidden labels from RAID or elsewhere
- [ ] Confirm remaining teammate identity and integrate team contributions
- [x] Upload Alma's incremental migration commits preserving the documented local development sequence
- [ ] Add Ali's final-model training/prediction instructions and Jennie's experiments
- [ ] Record exact Kaggle scores and submission IDs for the comparison
- [ ] Open one team PR from a writable course fork into sekhari/human-or-ai-submissions main
