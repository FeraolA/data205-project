# Traffic Violations Data Analysis
**DATA 205 | Feraol Abera | Spring 2026**

An end-to-end data analysis of traffic stop patterns in Montgomery County, Maryland using open government data. The project investigates how time of day, day of week, location, demographics, and stop circumstances relate to enforcement outcomes (warnings, citations, and arrests).

---

## Research Questions

| # | Question | Method |
|---|----------|--------|
| Q1 | How does time of day affect violation frequency? | Summary stats, bar chart |
| Q2 | Which violation types occur most frequently? | Frequency table, bar chart |
| Q3 | Does day of week relate to stop count and type? | Line chart, Chi-square test |
| Q4 | Does time of day affect warning vs. citation likelihood? | Logistic Regression |
| Q5 | Are certain locations tied to higher violation counts? | Geographic heatmap |
| Q6 | Do demographics relate to violation type? | Chi-square test, stacked bar |
| Q7 | What factors influence whether a search is conducted? | Logistic Regression |
| Q8 | Does search conducted relate to stop outcome? | Chi-square test |
| Q9 | Which violations have the highest arrest probability? | Grouped bar chart |
| Q10 | Can we predict stop outcome from key variables? | Random Forest classifier |

---

## Data Sources

| Dataset | Source | Size |
|---------|--------|------|
| [Traffic Violations](https://data.montgomerycountymd.gov/Public-Safety/Traffic-Violations/4mse-ku6q/about_data) | Montgomery County Open Data Portal | 1.4M+ records, ~35 variables |
| [Parking Garage and Lot Inventory](https://data.montgomerycountymd.gov/Transportation/Parking-Garage-and-Lot-Inventory/rd7s-ntxu/about_data) | Montgomery County Open Data Portal | ~200 records |

See [data/raw/DATA_ACCESS.md](data/raw/DATA_ACCESS.md) for download links and API endpoints. Raw datasets are not stored in this repository due to GitHub file size limits.

---

## Repository Structure

```
data205-project/
|-- data/
|   |-- raw/              # DATA_ACCESS.md with download links
|   |-- processed/        # Cleaned traffic violations dataset
|   `-- external/         # Cleaned parking inventory dataset
|
|-- ingestion/
|   |-- notebooks/        # 1.0-data-ingestion-cleaning.ipynb
|   `-- docs/             # cleaning_pipeline.md
|
|-- eda/
|   |-- notebooks/        # 1.1-eda-summary.ipynb
|   |-- images/           # All EDA charts
|   `-- docs/             # eda_findings.md
|
|-- analysis/
|   |-- notebooks/        # 2.0-final-analysis-modeling.ipynb
|   |-- images/           # Model output charts
|   `-- docs/             # analysis_interpretations.md
|
|-- models/               # Saved Random Forest model
|-- reports/              # Progress report and final report
`-- README.md
```

---

## How to Reproduce

All notebooks run in Google Colab with Python 3. Run them in order:

1. Open `ingestion/notebooks/1.0-data-ingestion-cleaning.ipynb` in Colab
2. Mount Google Drive when prompted
3. Run all cells -- this loads both datasets from the API, cleans them, and saves to Drive
4. Open `eda/notebooks/1.1-eda-summary.ipynb` -- reads the cleaned file and generates all EDA charts
5. Open `analysis/notebooks/2.0-final-analysis-modeling.ipynb` -- runs statistical models and saves outputs

**Dependencies** (installed automatically in each notebook):
- `pandas`, `numpy`, `matplotlib`, `seaborn` -- data manipulation and plotting
- `folium` -- interactive geographic heatmap
- `scipy` -- chi-square tests
- `scikit-learn` -- logistic regression and random forest
- `joblib` -- model serialization

---

## Key Findings

- Violations peak at 4 PM -- rush hour enforcement is highest, lowest volume at 4 AM
- Warnings dominate -- 67% of stops result in a warning, not a citation
- Tuesday is the busiest day -- weekday enforcement is significantly higher than weekends
- Searches are rare but predictive -- alcohol and accident involvement are the strongest predictors
- Geographic hotspots: Georgia Ave, Veirs Mill Rd, Colesville Rd in Silver Spring and Gaithersburg
- Chi-square p < 0.001 -- day of week and violation type are statistically associated

---

## Tools and Methods

- **Languages:** Python 3
- **Environment:** Google Colab + Google Drive
- **Statistical methods:** Chi-square test of independence, Logistic Regression, Random Forest classification
- **Visualization:** matplotlib, seaborn, folium

---

## Author

**Feraol Abera** | Montgomery College | DATA 205 | Spring 2026
