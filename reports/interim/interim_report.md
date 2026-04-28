# Interim Report
## Traffic Violations Data Analysis
**DATA 205 | Feraol Abera | Spring 2026**

---

## Project Summary

This interim report covers work completed through Week 11 of the DATA 205 project. The project analyzes traffic stop patterns in Montgomery County, Maryland using the Traffic Violations dataset from the Montgomery County Open Data Portal. The primary goal is to understand what factors shape the outcome of a traffic stop and to build a model capable of predicting those outcomes.

At this stage, data ingestion and cleaning are fully complete, and exploratory data analysis covering Questions 1 through 3 and Questions 5 and 6 has been finished. Analysis work covering Questions 4 and 6 through 10 is underway.

---

## Data

### Datasets Used

| Dataset | Source | Records Used | Notes |
|---------|--------|--------------|-------|
| Traffic Violations | data.montgomerycountymd.gov | 150,000 loaded via API | 1.4M+ total available |
| Parking Inventory | data.montgomerycountymd.gov | ~200 records | Geographic context |

### Key Cleaning Steps

The raw Traffic Violations dataset required substantial cleaning before analysis. The following steps were applied in the ingestion notebook:

- Column names standardized to lowercase with underscores
- date_of_stop parsed as datetime; time_of_stop parsed as a time object
- Derived temporal features created: year, month, month_name, day_of_week, hour, time_period
- Yes/No fields (accident, alcohol, search_conducted, belts, etc.) converted to 0/1 integers
- Categorical fields (violation_type, race, gender, arrest_type) standardized to Title Case
- GPS coordinates of (0.0, 0.0) replaced with NaN -- approximately 15% of records had invalid coordinates
- Dataset filtered to records from 2018 onward for consistency

The cleaned dataset contains approximately 212,000 rows and 35 columns and is saved to data/processed/traffic_violations_clean.csv.

---

## Exploratory Data Analysis

### Q1 -- Time of Day vs. Violation Frequency

Traffic violations peak at 4 PM (hour 16) with approximately 12,700 stops. A secondary peak appears between 7 and 10 AM during morning commute hours. The lowest volume is at 4 AM with approximately 1,500 stops.

Breaking stops down by time period: Morning has the most stops (70,447), followed by Afternoon (56,870), Night (50,592), and Evening (34,264). This pattern is consistent with traffic volume throughout the day -- enforcement is concentrated where and when traffic is heaviest.

### Q2 -- Most Frequent Violation Types

Warnings are the most common stop outcome by a large margin, accounting for 143,121 stops (67% of the sample). Citations follow at 61,262 stops (29%), and ESERO (electronic safety equipment repair orders) account for 7,790 stops (4%). This distribution was somewhat surprising and suggests officers exercise considerable discretion. The strong class imbalance between warnings and citations has implications for modeling and will require balanced class weighting.

### Q3 -- Day of Week vs. Stop Count

Tuesday has the most stops (40,427). Stop volume declines steadily through the week and reaches its lowest on Sunday (16,282). The drop from Friday to Saturday is the steepest single-day decline.

A chi-square test of independence confirmed that day of week and violation type are not independent (Chi2 = 2,560.74, df = 12, p < 0.0001). This means the distribution of warnings, citations, and ESEROs differs significantly across days of the week.

### Q5 -- Geographic Distribution

An interactive folium heatmap was generated from approximately 20,000 geolocated stops. The highest violation density appears along major corridors: Georgia Ave, Veirs Mill Rd, Colesville Rd, and University Blvd. The Silver Spring and Gaithersburg sub-agencies show the densest stop clusters. Rural areas in the north and west of the county show very sparse coverage.

These patterns suggest that enforcement is concentrated in high-traffic urban corridors, which is expected. Geographic location will be an important variable in modeling, particularly when interpreting demographic patterns that may reflect where officers are deployed rather than who is being targeted.

### Q6 -- Demographics and Violation Type

A chi-square test of violation type by race returned a statistically significant result (p < 0.05). Hispanic drivers have the highest citation proportion at 39.7%, compared to 23.7% for White drivers, 28.2% for Black drivers, and 20.0% for Asian drivers. Warning rates are highest for Asian drivers (76.8%) and White drivers (73.6%).

These associations are statistically significant but should be interpreted carefully. Geographic clustering and sub-agency deployment patterns likely explain much of the variation. No causal claims are made from this data alone.

### Additional Pattern -- Monthly Volume Trend

Stop volume has grown from approximately 5,000 per month in early 2024 to a peak of approximately 10,700 in October 2025. The apparent sharp drop in April 2026 reflects a partial month -- the dataset was pulled before the end of the month.

---

## How EDA Results Support the Analysis Plan

The EDA findings directly inform the modeling work planned for the analysis phase:

- The strong time-of-day signal from Q1 confirms that hour and time_period will be useful features in the logistic regression models for Q4 and Q7
- The class imbalance found in Q2 (warnings vs. citations) means all classifiers will need class_weight=balanced
- The statistically significant day-of-week effect from Q3 supports including dow_num as a feature in the Random Forest model for Q10
- The geographic clustering from Q5 reinforces the need to treat demographic associations from Q6 with caution and to control for sub-agency when possible
- The search-outcome relationship expected in Q8 is set up by Q6 findings -- groups with higher citation rates also tend to have different search rates

---

## Work Remaining

| Item | Description | Status |
|------|-------------|--------|
| Q4 | Logistic Regression -- warning vs. citation | In progress |
| Q7 | Logistic Regression -- search conducted | In progress |
| Q8 | Chi-square -- search vs. outcome | Not started |
| Q9 | Arrest probability by violation type | Not started |
| Q10 | Random Forest -- predict stop outcome | Not started |
| Final report | Synthesize all findings | Not started |
| Final presentation | Slides for final presentation | Not started |

---

## Challenges and Issues

- Approximately 15% of records had invalid GPS coordinates (0.0, 0.0) and had to be excluded from geographic analysis
- The full dataset exceeds GitHub's 100 MB file size limit and cannot be stored directly in the repository -- addressed via DATA_ACCESS.md with API links
- Strong class imbalance between violation types (67% warnings vs. 29% citations) will require careful handling in all classification models
- Demographic associations in Q6 require cautious interpretation -- confounding by geography and sub-agency is likely

---

## Repository Status

The GitHub repository is publicly accessible at github.com/FeraolA/data205-project and is organized as follows:

- ingestion/notebooks/ -- data loading and cleaning notebook complete
- eda/notebooks/ -- EDA notebook complete, all charts saved to eda/images/
- analysis/notebooks/ -- analysis notebook in progress
- ingestion/docs/, eda/docs/, analysis/docs/ -- written documentation for each phase
- data/processed/ -- cleaned traffic violations dataset
- data/external/ -- cleaned parking inventory dataset
- reports/ -- this interim report and the project plan
