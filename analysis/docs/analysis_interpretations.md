# Analysis Interpretations

**Notebook:** `analysis/notebooks/2.0-final-analysis-modeling.ipynb`

---

## Q4 -- Warning vs. Citation: Logistic Regression

**Method:** Logistic Regression predicting Citation (1) vs. Warning (0) using features: `hour`, `dow_num`, `search_conducted`, `alcohol`, `accident`.

- `alcohol` and `search_conducted` are the strongest positive predictors of a citation
- `accident` involvement also significantly increases citation likelihood
- `hour` has a modest effect -- afternoon stops are slightly more likely to result in citations

---

## Q6 -- Demographics vs. Violation Type: Chi-Square

**Method:** Chi-square test of independence for Race vs. Violation Type and Gender vs. Violation Type.

- Both tests return p < 0.05 -- statistically significant associations exist
- Hispanic drivers have the highest citation proportion (~39.7%); Asian and White drivers show higher warning rates (~73 to 77%)
- Gender differences are present but smaller in magnitude
- **Caveat:** Associations are correlational. Geographic clustering, sub-agency patterns, and traffic volume likely explain much of the variation. No causal claims are made.

---

## Q7 -- Search Conducted: Logistic Regression

**Method:** Logistic Regression with `class_weight='balanced'` to handle class imbalance (searches are rare events).

- `alcohol` is the strongest predictor of a search being conducted
- `accident` involvement also significantly increases search likelihood
- Violation type contributes -- certain categories are much more associated with searches
- `hour` has a minor effect on search probability

---

## Q8 -- Search Conducted vs. Stop Outcome: Chi-Square

**Result:** Statistically significant (p < 0.05). Stops where a search was conducted have a markedly different distribution of outcomes -- more citations and fewer warnings compared to stops without a search. This is consistent with Q4 findings linking searches to more serious enforcement actions.

---

## Q9 -- Arrest Probability by Violation Type

Violation types associated with alcohol and reckless driving have the highest observed arrest rates. Speeding and ESERO violations result mostly in warnings or citations with very low arrest rates. Only violation types with 100 or more stops were included to ensure statistical reliability.

---

## Q10 -- Random Forest: Predict Stop Outcome

**Method:** Random Forest Classifier with 200 trees, max_depth=12, balanced class weights. Trained on a 40,000-record sample for computational performance.

### Top Features by Importance

- `race_enc` and `gender_enc` -- capture enforcement pattern variation across sub-agencies and locations
- `hour` -- time of day strongly associated with stop outcome type
- `search_conducted` -- searches strongly predict more serious outcomes
- `alcohol` -- direct predictor of arrest-level outcomes
- `dow_num` and `period_num` -- temporal context contributes meaningfully

**Model performance:** Macro-averaged F1 typically 0.60 to 0.70. Warning and Citation classes perform best; ESERO is harder to predict due to smaller sample size.

**Saved model:** `models/random_forest_stop_outcome.joblib`

---

## Limitations

- Class imbalance between violation types required `class_weight='balanced'` in all classifiers
- Ordinal encoding of `day_of_week` and `time_period` imposes a ranking that may not reflect true relationships
- All findings are correlational -- no causal conclusions should be drawn
- Dataset capped at 150,000 records for performance; results may differ on the full 1.4M+ record dataset
- Demographic variables capture enforcement patterns but should not be interpreted as causal drivers of outcomes

---

## Images Generated

| File | Description |
|------|-------------|
| `q4_logreg_coefficients.png` | Coefficient plot -- Warning vs. Citation predictors |
| `q6_violation_by_race.png` | Stacked bar -- violation type by race (row %) |
| `q6_violation_by_gender.png` | Stacked bar -- violation type by gender (row %) |
| `q7_search_conducted_logreg.png` | Coefficient plot -- search conducted predictors |
| `q8_search_vs_outcome.png` | Stacked bar -- stop outcome by search conducted |
| `q9_arrest_rate_by_violation.png` | Horizontal bar -- arrest rate by violation type (top 10) |
| `q10_confusion_matrix.png` | Random forest confusion matrix on test set |
| `q10_feature_importance.png` | Random forest feature importance bar chart |
