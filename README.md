# Crank & Cadence
## Power BI  Portfolio Project
### Requirement Gathering
I instructed Claude to be the manager for a fictitious Bicycle shop **Crank&Cadence** so I can build a unique report and allow me to expand on and clarify exactly what questions the dashboard needs to answer, who the audience is, and what decisions it will support.

### Data Discovery and Extraction
For this project I utilised the Mavern Analytics AdventureWorks dataset. If this project was to begin with a database, I would utilize my **SQL** knowledge to collect all relevant data into their respective tables ready for the next stages.

### Data Cleaning
Removed duplicate CustomerIDs as I will use this as my Primary Key field, fixed data type issues resulting from US datasets working on a UK based Machine that included currency and date formats to ensure sales by month is calculated and displayed correctly, removed records with specific nulls from Customer Lookup as missing data will render the whole row unusable, and corrected any inconsistencies so the data is reliable and consistent using **Power Query**.

### Data transformation
Shaped and restructured the data into a usable format, such as creating calculated columns, merging Sales Data tables, and aggregating data to the right granularity, examples include:

- Expanded the Calendar Lookup Single Column Table to include weeks, months, quarters and years.
- Used Column Distribution, Quality and Profiling to assess and clean tables.
- Used the **DAX** SWITCH function to bucket customers based on income level.
- Returning to importing Sales Data via a folder rather than merging in **Power Query**, this will allow easier updating once a month by dropping the latest months data CSV into the folder and hitting refresh.

### Data modelling
Built the relationships between tables to create a structured **Star schema** that allowed measures to filter and interact correctly.

![Project Star Schema](<github files/assets/CnC Schema.png>)

### Measure creation
Wrote **DAX** measures and calculated fields to produce the KPIs and metrics the dashboard required.

- Created Hierarchies for Date and Territory for future drill-down functionality.
- Created Explicit Measures and organised them within a dedicated Measures Table.

### Sample of DAX used to create a usable clean visual
Gauge Charts used in the Product page to track each category’s monthly revenue target with a Visual Level filter to display the latest month's revenue in the Middle Value. A conditional format was used for the Middle Value to turn Red if the target hasn’t been reached. This brings attention allowing business action if needed.

![gauge chart](<github files/assets/Gauge Chart Example.png>)

#### DAX MEASURE #1
```dax
Total Revenue =
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] *
    RELATED(
        'Product Lookup'[ProductPrice]
    )
)
```

#### DAX MEASURE #2
```DAX
Accessories Revenue =
CALCULATE(
[Total Revenue],
'Product Categories Lookup'[ProductCategoryKey] = 4)
```
#### DAX MEASURE #3
```DAX
Accessories Previous Month Revenue =
CALCULATE(
    [Total Revenue],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    ),
    'Product Categories Lookup'[ProductCategoryKey] = 4
)
```

#### DAX MEASURE #4
```DAX
Accessories Revenue Target =
[Accessories Previous Month Revenue] * 1.1
```

#### DAX MEASURE #5
```DAX
Gap Target Accessories Revenue =
[Accessories Revenue] - [Accessories Revenue Target]
```
### Visualisation design
Carefully selected the appropriate chart types and built the visuals in a layout that is clear, intuitive, and tells a coherent story without repeating the data or overwhelming the target audience.

- Added Visualisation relationships to ensure correct interactivity behaviour between slicers and their corresponding visuals.

#### Page 1 - Exec Dashboard
Revenue, profit, orders and return rate KPI cards, year on year trend lines for revenue and profit, and a monthly orders bar chart gauge busy months and to compare this month's revenue to the same month last year.

![Exec Dash](<github files/assets/Exec Dash.png>)

#### Page 2 - Product Performance
Scatter plot of product revenue vs profit, top 10 products bar chart, gauges for quick high level category target tracking.

![Product Dash](<github files/assets/Product Dash.png>)

#### Page 3 - Customer Insights
Unique customers and revenue per customer KPIs, new vs returning donut chart, top 20 customers table, and demographic breakdowns.

![Customer Dash](<github files/assets/Customer Dash.png>)

#### Page 4 - Regional Detail
Bubble map to compare territories based on their revenue with a custom KPI tooltip to see detailed metrics without adding clutter.

![Territory Dash](<github files/assets/Territory Dash.png>)

### Data Validation
Cross checked numbers against known figures and the raw data in **Power Query** to ensure everything is calculating correctly.

### Stakeholder review
Presented the dashboard to the manager for feedback and to make any adjustments to ensure it meets the original requirements. Feedback was positive and the manager was pleased but after showing it to other stakeholders, the manager requested a way to be able to filter the whole table by continent so each Sales Rep can explore and keep control of their territories.

When hearing the Sales Reps will be using this report **I offered to make a Mobile Friendly version** to allow quick reference when out in the field. A Metric Selector was also added to the Customer page to allow users to toggle the Line Chart between “Total customers” and “Average Revenue Per Customer” to use one visual for two needs as two stakeholders wanted different visuals.

A Slicer panel was added to allow users to filter the report based on Year and Territory in a clear pop out Slicer Panel, this adds function but keeps the clean user experience.

![Slicer Panel](<github files/assets/Slicer Panel.png>)

As well as ensuring the whole report is Mobile friendly for the Sales Reps on the road who prioritise smaller screens such as tablets and smartphones.

![Mobile Layout](<github files/assets/Mobile Layout.png>)

### Problem and Solution
An example problem faced during building the visuals was the request to overlay revenue on a Line Chart to compare Months of this year vs the months from last year for a quick reference to tell we are busier than this time last year. 

#### Problem
Trying to overlay “This year's revenue” over “Last year's revenue” was not behaving the way it should due to the datasets most recent year was 2022, I was unable to use time intelligence and resulted in filtering based on year.

#### Solution
After trouble shooting with the help of **Claude** I was able to find that the root cause was that filtering through the Calendar Lookup relationship wasn't working reliably, filtering directly on Sales Data OrderDate provided me with reliable data which I verified using separate Matrix tables and **Power Query** summaries. **If an LLM was not available**, I would have diagnosed the issue by running each measure in a Matrix and studied the behaviour and outputs as a process of elimination until I found and resolved the troubling filter.

### Summary
This project was to separate away from a mainstream Power BI course and put a unique and personal touch to it. Using **Claude** as an interactive manager, I was able to drill down on the “What will you use this for?” and “What would you like to learn from this?” questions, get to the root questions to **ensure a rich and useful report** supplemented with **strong user experience design** guarantees this report will be used and enjoyed by the stakeholders with key metrics and a non-overwhelming design layout.

### Next steps
If the feature was available to me, I would publish this report to the Power BI service, set up **User Roles** based on territory and set up **live data sourcing** and **automatic refreshes** to ensure data is always current, removing reliance on manual updates.

### Thank you
Edward Ford
