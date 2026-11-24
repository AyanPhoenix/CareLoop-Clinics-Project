# CareLoop Clinics — No-Show Guard

**GitHub Repository:** [https://github.com/<your-username>/<your-repo-name>]

## Summary
- **Part 1 (Data & Exploratory Analysis):** [Insert key finding from data cleaning, reminder/no-show rates, or feature distributions].
- **Part 2 (Baseline & Feature Engineering):** [Insert key finding from predictor relationships, odds ratios, or engineered features].
- **Part 3 (Predictive Modeling & Tuning):** [Insert key finding from model comparison, GridSearchCV tuning, or threshold 0.35 trade-off].
- **Part 4 (GenAI Call-Note Insights):** [Insert key finding from structured root-cause extraction, prompt V1 vs. V2, or reminder cross-tabulation].

## Recommendation
To reduce missed clinic appointments efficiently, CareLoop Clinics should target high-risk bookings using the tuned model at the 0.35 probability threshold paired with tailored follow-ups—such as pre-visit reminders for `forgot_or_no_reminder` and scheduling/transport support for other top root causes. For example, across our analysis, [X]% of analysed notes pointed to [top root cause], the 0.35 threshold flagged [Y]% of cases compared to [Z]% at 0.50 (raising recall to [R]%), and the test set achieved a ROC-AUC of [A] across [N] records—allowing front-desk staff to focus outreach capacity where no-show risk and actionable follow-ups overlap most.

## Chosen Model & Threshold Details
- **Selected Model:** Tuned `RandomForestClassifier` selected via 5-fold `GridSearchCV` (`scoring="roc_auc"`) over `n_estimators` and `max_depth` on the training split (compared alongside Logistic Regression with a fold-level `StandardScaler` pipeline, a depth-3 Decision Tree, and an untuned Random Forest).
- **Test ROC-AUC:** `[insert test_risk_scores ROC-AUC value]` (evaluated on continuous `predict_proba` scores).
- **Decision Threshold (`0.35` vs. `0.50`):** Out-of-fold training probabilities (`cross_val_predict`) were evaluated at `0.50` and `0.35`. Threshold **`0.35`** was chosen over `0.50` to capture higher recall on likely no-shows within operational follow-up capacity, trading some precision/specificity for fewer missed no-shows.

## Limitations
1. **Observational Data Associations:** Historical clinic and call-centre records capture correlations rather than causal effects—unobserved factors (such as patient urgency or exact travel disruptions) may drive both reminders and attendance.
2. **Train/Test Split & Temporal/Generalization Constraints:** A single train/test split may not capture seasonal shifts, clinic-specific workflow differences, or changes in patient behavior over time.
3. **Sample Size & Note Coverage Constraints:** A limited number of call-centre follow-up notes (including blank or low-confidence entries) restricts statistical power when cross-tabulating specific root causes against `reminder_sent`.
