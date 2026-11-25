# Submission — Answers Summary

The uploaded ZIP contains one file: `Submission/Answers.docx`. This Markdown file is a searchable summary of the answers and outputs extracted from that document; it is not a replacement for the original Word document.

## Part 2 — SQL

- Database: `noshowguard`.
- The duplicate-insert test for appointment `A0001` returned MySQL error 1062 because that primary key already exists.
- Clinic summary identified clinic C08 as the highest no-show rate: 33.3% (30 no-shows from 90 appointments).
- Lead-time bands use inclusive ranges: 1–7, 8–14, 15–30, and 31+ days.
- Monthly analysis uses `LAG()` to calculate month-to-month percentage-point change.
- City ranking uses `RANK() OVER (PARTITION BY city ORDER BY no_show_pct DESC)`.

## Part 3 — Predictive modelling

- Train/test split: 792 training rows and 198 test rows; the test set contains 49 no-shows.
- Baseline accuracy (always predict attended): 75.25%.
- Logistic Regression test results reported in the document: accuracy 78.79%, recall 34.69%, ROC-AUC 0.7889.
- Depth-3 Decision Tree: training accuracy 82.20%, test accuracy 77.78%; unrestricted tree: 100.00% training and 67.17% test accuracy.
- Random Forest: test accuracy 74.75%, recall 30.61%, ROC-AUC 0.7057.
- Random Forest grid search: best parameters `max_depth=3`, `n_estimators=200`; best CV ROC-AUC 0.7673.
- Mean 5-fold CV ROC-AUC: Logistic Regression pipeline 0.7737; depth-3 tree 0.6784; Random Forest 0.7466; tuned Random Forest 0.7673.
- Out-of-fold threshold comparison: 0.50 gives precision 57.53%, recall 24.85%, F1 0.3471 and 73 flagged; 0.35 gives precision 51.30%, recall 46.75%, F1 0.4892 and 154 flagged.
- The document reports threshold 0.35 test precision 57.14%, recall 57.14%, 49 flagged and ROC-AUC 0.7889.
- Reminder odds-ratio calculations reported: exponentiated scaled coefficient 0.5595; corrected No-to-Yes odds ratio 0.3094.

## Part 4 — Gemini and Pydantic

- The Pydantic model validates `root_cause`, `confidence` (0–1), `summary`, and `recommended_action`.
- Allowed root causes in Task 4.2: `forgetfulness`, `transport`, `scheduling_conflict`, `illness`, `work_or_family_commitment`, `cost`, `other`, and `unclear`.
- The notebook code processes the first 10 rows of `followup_notes.csv`, stores the raw response, validates the JSON and allowed category, and writes `notes_analysed.csv`.
- Task 4.3 compares prompts for note IDs N007 and N009. Its code currently sets the allowed-category list to only `other` (with placeholder comments for the remaining categories), which should be checked against the assignment's required category list.
- Task 4.3(c) initializes `api_call_count = 0` and defines a counted wrapper; as written in the document, it prints the count without showing calls routed through that wrapper, so verify the counter logic before relying on the total.

## Files not present in the ZIP

The ZIP does not include `appointments_clean.csv`, `notes_analysed.csv`, `test_risk_scores.csv`, notebooks, or other generated outputs. This summary does not fabricate those missing files or the reminder/root-cause cross-tab.
