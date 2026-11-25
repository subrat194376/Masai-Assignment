# Clinic Appointment No-Show Analysis

**Public GitHub repository:** https://github.com/subrat194376/Masai-Assignment

## Four-part summary

- **Part 1 – Data cleaning:** The raw appointment file contained 1,020 rows; after removing 20 exact duplicates and 10 rows missing a usable appointment date or status, 990 appointments remained, including 218 no-shows (22.0%).
- **Part 2 – SQL analysis:** Appointments booked 31+ days ahead had a 60.5% no-show rate (26/43), compared with 16.2% (63/390) for appointments booked 1–7 days ahead.
- **Part 3 – Predictive modelling:** Logistic Regression with a scaling pipeline was selected (5-fold training CV ROC-AUC 0.7737); at threshold 0.35, holdout ROC-AUC was 0.7889 and 49 appointments were flagged.
- **Part 4 – Follow-up notes:** The Gemini/Pydantic exercise uses a 10-note sample; its root-cause labels and reminder cross-tab are exploratory and should not be interpreted as clinic-wide prevalence. The final analysed-notes output is not yet included in this repository.

## Business recommendation

The clinic should prioritize earlier confirmation and targeted outreach for appointments booked far in advance, while using a risk score to help allocate follow-up capacity. In the cleaned dataset, 218 of 990 appointments were no-shows (22.0%); the 31+ day lead-time group had a 60.5% no-show rate versus 16.2% in the 1–7 day group. Clinic C08 also had the highest observed no-show rate at 33.3% (30/90). For appointment-level prioritization, the selected Logistic Regression pipeline achieved a 5-fold training ROC-AUC of 0.7737 and a holdout ROC-AUC of 0.7889; a 0.35 probability threshold flagged 49 holdout appointments and achieved 57.1% precision and 57.1% recall. The clinic should pilot reminders and confirmation calls for the highest-risk appointments, then review outcomes and adjust the threshold to match actual call capacity.

## Model selection

The selected model was a **StandardScaler + LogisticRegression pipeline**. It had the highest mean 5-fold training ROC-AUC among the compared candidates (0.7737), narrowly exceeding the tuned Random Forest (0.7673). The operating threshold was **0.35**, selected because it gave a higher out-of-fold F1 score than 0.50 (0.4892 versus 0.3471). On the held-out test set, the model's ROC-AUC was **0.7889**.

## Limitations

1. The cleaning statistics and outlier bounds were calculated using all usable rows before the train/test split, so information from the test portion influenced preprocessing.
2. The same patient can appear in both training and test sets, so performance may be optimistic for genuinely unseen patients.
3. The 0.35 threshold is a modelling choice; the practical threshold should depend on the clinic's available call capacity and the cost of missed no-shows versus unnecessary calls.

## Reproducing the reminder/root-cause cross-tab

Run the following after `notes_analysed.csv` has been generated:

```python
import pandas as pd

notes = pd.read_csv("notes_analysed.csv")
appointments = pd.read_csv("appointments_clean.csv")

merged = notes.merge(
    appointments[["appointment_id", "reminder_sent"]],
    on="appointment_id",
    how="left",
    validate="many_to_one"
)

print(pd.crosstab(
    merged["root_cause"],
    merged["reminder_sent"],
    margins=True,
    dropna=False
))
```

## Security

Do not commit API keys, `.env` files, credentials, access tokens, or notebook outputs containing secrets. Keep API keys in environment variables or enter them interactively at runtime.
