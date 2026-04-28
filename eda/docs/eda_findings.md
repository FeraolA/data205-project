# EDA Findings Summary

**Notebook:** `eda/notebooks/1.1-eda-summary.ipynb`

---

## Q1 -- Time of Day vs. Violation Frequency

Violations peak at 4 PM (hour 16) with approximately 12,700 stops, aligning with afternoon rush hour. A secondary peak occurs between 7 and 10 AM during the morning commute. The lowest volume is at 4 AM with around 1,500 stops.

By time period: Morning has the most stops (70,447), followed by Afternoon (56,870), Night (50,592), and Evening (34,264).

**Interpretation:** Enforcement activity closely mirrors traffic volume. Officers are most active during commute hours, which is consistent with expected resource allocation patterns.

---

## Q2 -- Most Frequent Violation Types

Warnings dominate at 143,121 stops (67%), followed by Citations at 61,262 (29%), and ESERO at 7,790 (4%).

**Interpretation:** Officers use significant discretion -- most stops do not result in formal enforcement action. This class imbalance between warnings and citations must be accounted for in any predictive model.

---

## Q3 -- Day of Week vs. Stop Count

Tuesday has the most stops (40,427) and volume steadily declines through the week, reaching its lowest on Sunday (16,282). A chi-square test confirmed that day of week and violation type are not independent (Chi2 = 2,560.74, df = 12, p < 0.0001).

**Interpretation:** Weekday enforcement is significantly higher than weekend enforcement. The type of stop also varies by day, with weekdays showing proportionally more citations.

---

## Q5 -- Geographic Patterns

The interactive heatmap (`eda/images/q5_geographic_heatmap.html`) shows violation density concentrated along major corridors: Georgia Ave, Veirs Mill Rd, Colesville Rd, and University Blvd. Silver Spring and Gaithersburg sub-agencies show the densest stop clusters. Rural areas in the north and west of the county have sparse enforcement.

**Interpretation:** Enforcement concentrates in high-traffic urban corridors. Location is an important variable for modeling and must be considered when interpreting demographic patterns.

---

## Additional Patterns

### Monthly Volume Trend

Stop volume grew from approximately 5,000 per month in early 2024 to a peak of around 10,700 in October 2025. The sharp drop in April 2026 reflects an incomplete month -- data is still being recorded.

### Violation Type by Race

Hispanic drivers have the highest citation rate (39.7%) compared to other groups (20 to 28%). Warning rates range from 55 to 77% across groups. The association is statistically significant (p < 0.05). These differences likely reflect geographic and sub-agency factors rather than direct causal relationships. No causal claims are made.

---

## Images Generated

| File | Description |
|------|-------------|
| `q1_violations_by_hour.png` | Hourly stop count bar chart -- peak at 4 PM highlighted in red |
| `q1_violations_by_period.png` | Stop count by time period with exact values |
| `q2_top_violation_types.png` | Horizontal bar chart -- Warning 143K, Citation 61K, ESERO 7.8K |
| `q3_stops_by_day_of_week.png` | Line chart with fill -- Tuesday peak, Sunday low |
| `q5_geographic_heatmap.html` | Interactive folium heatmap of violation locations |
| `monthly_volume_trend.png` | Monthly time series 2024 to 2026 |
| `violation_type_by_race_heatmap.png` | Heatmap showing violation proportions by race group |
