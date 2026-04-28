# Project Plan
## Traffic Violations Data Analysis
**DATA 205 | Feraol Abera | Spring 2026**

---

## Project Overview

This project analyzes traffic stop patterns in Montgomery County, Maryland using publicly available data from the Montgomery County Open Data Portal. The goal is to understand what factors influence the outcome of a traffic stop, whether that is a warning, citation, or arrest, and to build a model that can predict those outcomes from stop characteristics.

The project uses two datasets: the Traffic Violations dataset with over 1.4 million records and approximately 35 variables, and the Parking Garage and Lot Inventory dataset for geographic context.

---

## Research Questions

1. Q1: How does time of day affect the frequency of traffic violations?
2. Q2: Which violation types occur most frequently?
3. Q3: Does day of week relate to stop count and violation type?
4. Q4: Does time of day affect whether a stop results in a warning vs. citation?
5. Q5: Are certain geographic locations associated with higher violation counts?
6. Q6: Do driver demographics relate to violation type?
7. Q7: What factors influence whether a search is conducted during a stop?
8. Q8: Does whether a search was conducted relate to the stop outcome?
9. Q9: Which violation types are associated with the highest arrest probability?
10. Q10: Can we predict stop outcome from key stop characteristics?

---

## Data Sources

| Dataset | Source | Size | Role |
|---------|--------|------|------|
| Traffic Violations | Montgomery County Open Data Portal | 1.4M+ records, ~35 variables | Primary |
| Parking Garage and Lot Inventory | Montgomery County Open Data Portal | ~200 records | Secondary |

Both datasets are accessed via the Socrata Open Data API. The raw datasets exceed GitHub's 100 MB file size limit and are not stored directly in the repository. A DATA_ACCESS.md file in data/raw/ provides direct download links and API endpoints.

---

## Analytical Methods

### Exploratory Data Analysis (Q1, Q2, Q3, Q5, Q6)

- Summary statistics and frequency counts for all key variables
- Bar charts and line charts for time-based patterns (hour, day of week, month)
- Chi-square test of independence for Q3 (day of week vs. violation type) and Q6 (demographics vs. violation type)
- Interactive geographic heatmap using folium for Q5

### Statistical Modeling (Q4, Q7, Q8)

- Logistic Regression to predict warning vs. citation (Q4) and search conducted (Q7)
- Chi-square test for Q8 (search conducted vs. stop outcome)

### Machine Learning (Q9, Q10)

- Arrest probability analysis by violation type using grouped bar chart (Q9)
- Random Forest classifier to predict stop outcome from stop characteristics (Q10)
- Feature importance analysis to identify top predictors

---

## Tools and Environment

| Tool | Purpose |
|------|---------|
| Python 3 | Language |
| Google Colab + Google Drive | Environment |
| pandas, numpy | Data wrangling |
| matplotlib, seaborn, folium | Visualization |
| scipy | Chi-square tests |
| scikit-learn | Logistic Regression, Random Forest |
| joblib | Model saving |

---

## Repository Structure

| Folder | Contents |
|--------|----------|
| ingestion/notebooks/ | Data loading and cleaning notebook |
| eda/notebooks/ | Exploratory data analysis notebook |
| analysis/notebooks/ | Statistical models and machine learning notebook |
| data/processed/ | Cleaned traffic violations dataset |
| data/external/ | Cleaned parking inventory dataset |
| eda/images/ | EDA charts and geographic heatmap |
| analysis/images/ | Model output charts |
| models/ | Saved Random Forest model (.joblib) |
| reports/ | Project plan, progress report, and final report |

---

## Timeline

| Week | Milestone |
|------|-----------|
| Week 9 | Data loading, cleaning, ingestion pipeline complete |
| Week 10 | EDA complete -- all Q1 to Q5 charts and chi-square tests done |
| Week 11 | Progress report and presentation |
| Week 12 | Statistical models complete (Q4, Q6, Q7, Q8) |
| Week 13 | Machine learning complete (Q9, Q10), model saved |
| Week 14 | Final report written, repository finalized |
| Week 15 | Final presentation |

---

## Expected Outcomes

By the end of the project, the analysis will have produced answers to all ten research questions, a trained Random Forest model saved in the repository, a full set of visualizations saved to eda/images/ and analysis/images/, and written interpretations of all findings in the docs folders. The final report will synthesize all results into a clear narrative about traffic enforcement patterns in Montgomery County.
