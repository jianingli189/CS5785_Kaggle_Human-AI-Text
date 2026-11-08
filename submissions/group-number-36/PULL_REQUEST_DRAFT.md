# Draft title

[Submission] group-number-36 — ly462, jl4837, [Abishek NetID pending]

## Team and scope

- Liyan (Alma) Yang (ly462): Random Forest experiments and candidate predictions.
- Jianing Li (jl4837): SVM, TF/MiniLM fusion, word/character feature optimization and calibrated candidate predictions.
- Abishek: official full name and NetID pending; implementation not yet integrated.
- Team name: Group Number 36. The report must use the eventual **course PR URL**, not just this team branch.

## Included evidence

Alma's RF package is under `alma_rf/`; Jianing's SVM package is under `jianing_svm/`. Both use the same 8,000/2,000 stratified split. Alma selected the style-only RF through training-only CV (validation 0.989913). Jianing selected word/character TF-IDF + MiniLM(0.25), SVM C=1 on validation (0.974103), then performed a training-only CV stability check. The selection protocols differ and are stated explicitly.

Public scores: Abishek 0.92847 and Alma 0.89682 from the previously supplied screenshot record; Jianing's updated 0.90456 was reported by Jianing on 2026-11-08. Earlier Jianing public 0.84249 is retained as history. None is a private score. Abishek remains the highest recorded public candidate, not a locked final version.

## Reproduction

See README for per-member working directories and separate environments.

- Alma validation/full training: `alma_rf/train.py`; probability prediction: `alma_rf/predict.py`.
- Jianing experiments: `jianing_svm/notebooks/01_Jianing_TF_MiniLM_SVM.ipynb` or `src/run_jianing_notebook.py` from that member directory.
- Jianing optimized probabilities after generating features: `python src/jianing_optimized_submission.py` from `jianing_svm/`.
- Original-source receipts, all candidate scores, selected configurations, figures, and final member CSVs are included. Raw data, embeddings, caches and checkpoints are excluded.

## Before opening the final course PR

- [x] All repository changes remain inside submissions/group-number-36/.
- [x] Alma and Jianing code, environment specifications, candidate CSVs and comparison evidence included.
- [x] Same validation ID membership verified for Alma and Jianing.
- [x] Current public scores distinguished from validation/private scores and attributed to their sources.
- [ ] Add Abishek's implementation and verify his split; confirm official identity.
- [ ] Complete learning-curve and unlabeled-data experiments.
- [ ] Select and document final team model; perform matching final-model error analysis.
- [ ] Complete report, consolidated AI-use disclosure, submission identifiers and final selection.
- [ ] Open one team PR into the recorded course destination sekhari/human-or-ai-submissions main, preserving meaningful history.
