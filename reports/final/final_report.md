# Final Report
## Traffic Violations Data Analysis
**DATA 205 | Feraol Abera | Spring 2026**

---

## Introduction and Project Overview

This project analyzes traffic stop patterns in Montgomery County, Maryland using publicly available data from the Montgomery County Open Data Portal. The goal was to understand what factors shape the outcome of a traffic stop including whether a driver receives a warning, a citation, or an arrest, and to build a model capable of predicting those outcomes from stop characteristics.

The project was organized around ten research questions ranging from descriptive (when do most stops happen?) to inferential (do demographics associate with violation type?) to predictive (can a machine learning model predict stop outcome?).

---

## Datasets

**Primary: Traffic Violations**
- Source: data.montgomerycountymd.gov
- 1.4 million total records; 150,000 loaded via Socrata API
- ~35 variables per stop including date, time, GPS location, violation type, driver demographics, arrest type, and whether a search was conducted
- Date range: 2024 to 2026 (filtered to 2018+ for analysis)

**Secondary: Parking Garage and Lot Inventory**
- Source: data.montgomerycountymd.gov
- ~200 parking facilities with location, capacity, and payment information
- Used for geographic proximity analysis alongside traffic stop locations

Both datasets are accessed via the Socrata Open Data API. Raw files exceed GitHub's 100 MB limit and are not stored in the repository. Direct download links are in data/raw/DATA_ACCESS.md.

---

## Goals

- **Descriptive:** Characterize when, where, and what kind of stops occur (Q1, Q2, Q3, Q5)
- **Inferential:** Test statistical associations between stop circumstances, demographics, and outcomes using chi-square tests and logistic regression (Q4, Q6, Q7, Q8)
- **Predictive:** Build a Random Forest classifier to predict stop outcome and identify the strongest predictors (Q9, Q10)

---

## Tools

| Tool | Purpose |
|------|---------|
| Python 3, Google Colab | Language and environment |
| pandas, numpy | Data loading and cleaning |
| matplotlib, seaborn | Static visualizations |
| folium | Interactive geographic heatmap |
| scipy | Chi-square tests |
| scikit-learn | Logistic Regression, Random Forest |
| joblib | Saving the trained model |

All code is organized across three Jupyter notebooks: ingestion, EDA, and analysis.

---

## Summary of Data Cleaning

The raw dataset required significant preparation:

- Column names standardized to lowercase with underscores
- date_of_stop parsed as datetime; time_of_stop as a time object
- Six temporal features derived: year, month, month_name, day_of_week, hour, time_period
- Yes/No fields (accident, alcohol, search_conducted, etc.) converted to 0/1 integers
- Categorical fields standardized to Title Case; invalid values replaced with NaN
- GPS coordinates of exactly (0.0, 0.0) set to NaN (approximately 15% of records had invalid location data)
- Dataset filtered to 2018 onward, leaving approximately 212,000 cleaned rows

Full cleaning code is in ingestion/notebooks/1.0-data-ingestion-cleaning.ipynb on GitHub.

---

## Basic Descriptive Statistics

| Metric | Value |
|--------|-------|
| Cleaned records | ~212,000 |
| Warnings | 143,121 (67%) |
| Citations | 61,262 (29%) |
| ESERO | 7,790 (4%) |
| Peak hour | 4 PM (hour 16) |
| Busiest day | Tuesday (40,427 stops) |
| Quietest day | Sunday (16,282 stops) |
| Morning stops | 70,447 |

The most striking baseline finding was that 67% of stops end in a warning rather than a citation. Officers exercise significant discretion at every stop.

---

## Description of Final Data Product

The final data product is a reproducible three-notebook analysis pipeline plus a trained Random Forest classifier.

**Notebooks:**
- The ingestion notebook loads both datasets from the API, applies all cleaning steps, and saves output files to Google Drive
- The EDA notebook produces seven charts covering time patterns, violation type distribution, geographic density, demographic breakdowns, and monthly volume trends
- The analysis notebook runs two logistic regression models, chi-square tests, arrest rate analysis, a parking proximity analysis, and a Random Forest classifier

**Parking proximity analysis:** Using the Parking Inventory dataset, I parsed coordinates for all ~200 facilities, overlaid them on the violation heatmap using folium, and used a BallTree to compute the distance from each traffic stop to its nearest parking facility. Stops were grouped into within 0.5 miles and farther away. A chi-square test found that stops near parking facilities have a significantly higher citation rate (p < 0.05), suggesting more formal enforcement near commercial areas.

**Random Forest model:** Trained on 40,000 records with 200 trees, max depth of 12, and balanced class weights. Predicts stop outcome (Warning, Citation, or ESERO) from ten features. The model is saved as models/random_forest_stop_outcome.joblib and can be reloaded without rerunning the full analysis.

**Value provided:** The entire pipeline is reproducible from a single API call. Anyone can clone the repository, open the notebooks in Colab, and go from raw data to trained model without downloading any files manually.

---

## Summary of Data Story

**Warnings dominate.** 67% of stops end in a warning. This was unexpected and suggests officers use substantial discretion. Most traffic stops are not formal enforcement actions.

**Time and place are highly predictable.** Stop volume peaks at 4 PM and is lowest at 4 AM. Tuesday has the most stops. The geographic heatmap shows that the vast majority of stops cluster along Georgia Ave, Veirs Mill Rd, Colesville Rd, and University Blvd. Rural areas have almost no enforcement presence.

**Day of week and violation type are statistically linked.** A chi-square test (Chi2 = 2,560.74, df = 12, p < 0.0001) confirmed that the distribution of warnings, citations, and ESEROs differs significantly across days of the week.

**Searches are rare but consequential.** Alcohol involvement is the strongest predictor of whether a search is conducted. Stops with a search have a dramatically different outcome distribution resulting in far more citations and far fewer warnings (chi-square p < 0.05).

**Proximity to parking matters.** Stops within 0.5 miles of a parking facility have a higher citation rate, pointing to more formal enforcement near commercial areas.

**Demographic associations exist but require careful interpretation.** Hispanic drivers have the highest citation proportion at 39.7%, compared to 23.7% for White drivers and 28.2% for Black drivers. This association is statistically significant but geographic clustering and sub-agency deployment patterns likely explain much of the variation. No causal claims are made.

**The Random Forest model performs reasonably well.** Top features by importance are race encoding, hour, gender encoding, search conducted, and alcohol. Macro-averaged F1 is approximately 0.60 to 0.70. Warning and Citation classes are predicted well; ESERO is harder due to smaller sample size.

Full charts, chi-square outputs, logistic regression coefficients, and confusion matrices are in the Jupyter notebooks on GitHub.

---

## What the Experience Was Like

**What was challenging:**

The biggest technical challenge was data scale. The full dataset has 1.4 million records, too large for GitHub and slow in Colab. Balancing data size with runtime required experimentation A 150,000-record API call ordered by date worked well enough to be representative. Interpreting the demographic analysis responsibly was also difficult. The chi-square results are statistically significant, but distinguishing a real pattern from geographic confounding required careful wording throughout.

**What was rewarding:**

Seeing the folium map render with real violation data and parking facility markers overlaid was satisfying and the geographic pattern was immediately readable. Building a fully reproducible pipeline where someone can clone the repository and run three notebooks to go from raw API data to a trained model felt like meaningful work. The parking proximity analysis was also rewarding because it connected both datasets in a concrete, testable way rather than treating the secondary dataset as an afterthought.

**What might I change:**

With more time, I would pull the full 1.4 million records rather than 150,000. Including subagency as a feature in all models would likely absorb much of the geographic and demographic variation and improve interpretability. A Tableau or Plotly Dash dashboard would also make the findings accessible to non-technical audiences.

---

## Next Steps and Recommendations

- Pull the full 1.4 million records and retrain all models
- Include subagency as a feature to control for geographic deployment patterns
- Run a time-series analysis to track whether enforcement patterns change year over year on specific corridors
- Build an interactive dashboard for public-facing findings
- For policymakers: compare the geographic concentration of stops with actual traffic volume and crash data to evaluate whether enforcement is proportionate

---

## Acknowledgements

Thank you to Professor Alraee for guidance throughout the semester and for the project structure that kept the repository organized. Thank you to the Montgomery County government for making the Traffic Violations and Parking Inventory datasets publicly available through the Open Data Portal.

---

## References

- Montgomery County Open Data Portal. Traffic Violations. Available at: https://data.montgomerycountymd.gov/Public-Safety/Traffic-Violations/4mse-ku6q/about_data
- Montgomery County Open Data Portal. Parking Garage and Lot Inventory. Available at: https://data.montgomerycountymd.gov/Transportation/Parking-Garage-and-Lot-Inventory/rd7s-ntxu/about_data
