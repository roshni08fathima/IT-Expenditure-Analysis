# IT Expenditure Analysis: Power BI Dashboard

An interactive Power BI dashboard that compares **actual, forecast and planned IT spend** across regions, business areas and cost categories, to show where budgets and reality don't match.

![Dashboard overview](screenshots/dashboard.png)

**Demo video:** [Watch the 5-minute walkthrough](ADD-YOUR-VIDEO-LINK-HERE)

---

## Business problem

How can an organization monitor and optimize its IT expenditure effectively? This dashboard supports strategic budgeting and operational accountability by showing:

- How IT costs are distributed across regions, business areas and cost elements
- The variance between actual, forecast and planned spend
- The cost drivers within labor, shared services and individual IT sub-areas

## Dataset

- **Source file:** `IT_Expenditure_dataset.xlsx`
- **Main sheet ("Data"):** about 81,600 rows and 12 columns: Month, Business Area, Region, Country, IT Sub Area, IT Area, Cost Element Name, Cost Element Group, Cost Element Sub Group, Actual, Forecast, Plan
- **Coverage:** 6 regions, 7 business areas, 5 IT areas, 38 IT sub-areas
- **Structure:** each row holds only one of Actual, Forecast or Plan, so blank values are by design and were not removed
- **"Future Data" sheet:** 12,100 rows with Actual values for Sep to Dec. Its origin is unverified, so it was loaded separately and **excluded** from the analysis

## Tools

Power BI Desktop (Power Query, DAX, report design) and Excel (source data)

## Data preparation (Power Query)

- Loaded the Excel data and renamed fields for consistency (`Date` to `Month`, `Cost element name` to `Cost Element Name`)
- Added a **Month Number** column and used it to sort months Jan to Dec
- Kept blanks in Actual, Forecast and Plan, because they reflect the data structure rather than missing values
- Kept 1,929 exact duplicate rows, since they may be separate postings
- Loaded the unverified "Future Data" sheet as a separate table

## Key DAX measures

```
Total Actual = SUM(IT_Spend[Actual])
Total Forecast = SUM(IT_Spend[Forecast])
Total Plan = SUM(IT_Spend[Plan])

Plan (Actual Months) =
SUMX(
    VALUES(IT_Spend[Month Number]),
    IF([Total Actual] <> 0, [Total Plan], 0)
)

Variance Actual vs Plan = [Total Actual] - [Plan (Actual Months)]
Variance % Actual vs Plan = DIVIDE([Variance Actual vs Plan], [Plan (Actual Months)])
Variance Forecast vs Plan = [Total Forecast] - [Total Plan]
Variance % Forecast vs Plan = DIVIDE([Variance Forecast vs Plan], [Total Plan])
```

`Plan (Actual Months)` compares actuals only with the plan for months that have actuals (Jan to Aug). Comparing 8 months of actuals with a 12-month plan would overstate the underspend.

## Dashboard pages

| Page | Purpose |
|---|---|
| **Dashboard** | KPI cards, slicers (Region, Business Area, Month) and the main charts |
| **EDA** | Exploratory visuals: monthly trend, spend by region, concentration of costs |
| **Drilldown** | Labor and shared services splits, biggest over and underspends, decomposition tree |

## Key insights (Jan to Aug actuals)

1. **Spend is 9.1% under plan:** 555.7M actual against a plan of 611.3M (-55.6M).
2. **The USA drives the gap:** it accounts for about -53.1M of the variance, while Europe is almost exactly on plan.
3. **R&D (-21%), Enablement (-19%) and BU (-17%)** underspend the most against plan.
4. **External labor is 9.5% under plan,** a larger gap than internal labor, which points to delayed or reduced contractor spend.
5. **Overspends hide inside the underspend:** Infrastructure "Other" costs are about 159% over plan, and Networking is about 4.7M over.
6. **Africa & Asia has actual spend but no plan,** which is a budgeting gap.
7. **The forecast reverses the trend:** from July to December the forecast runs about 9% above plan.

## Recommendations

- Rebudget areas that consistently underspend (R&D, Enablement, BU)
- Investigate the overspends in Infrastructure "Other" and Networking
- Add a plan for Africa & Asia so its spend can be monitored
- Validate the second-half forecast, which runs well above plan
- Review the April USA "Adjustments" entries (+10.4M, -68.9M, +58.2M), which net to about zero and look like allocation entries

## Limitations

- Actuals cover January to August only, so variances use the matching plan months
- The "Future Data" sheet is unverified and excluded
- The dataset has no year field, so there is no year-over-year comparison

## Repository structure

```
IT-Expenditure-Analysis/
├── IT_Expenditure_Analysis.pbix
├── IT_Expenditure_Dashboard.pdf
├── IT_Expenditure_Theme.json
├── data/
│   └── IT_Expenditure_dataset.xlsx
├── screenshots/
│   ├── dashboard.png
│   ├── eda.png
│   └── drilldown.png
└── README.md
```

## How to open

1. Download `IT_Expenditure_Analysis.pbix`
2. Open it in **Power BI Desktop** (free, Windows)
3. If prompted, point the data source to `data/IT_Expenditure_dataset.xlsx`

## Author

**Roshni Kousar Fathima**
[LinkedIn](ADD-YOUR-LINKEDIN-URL-HERE)
