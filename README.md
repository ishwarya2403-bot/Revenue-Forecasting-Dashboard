# Revenue-Forecasting-Dashboard
Revenue Forecasting &amp; Discount Analysis using Python and Tableau — cleaned and prepared sales data with Python and built interactive Tableau dashboards for revenue forecasting, actual vs forecast comparison, and discount impact analysis.

# Revenue Forecasting & Discount Analysis

## Project Overview

This project focuses on analysing historical sales data and building interactive dashboards to understand revenue performance, forecast future revenue, and analyse the impact of discounts.

The project uses **Python for data cleaning and data preparation** and **Tableau for data visualization and forecasting**.

The main objective is to transform historical sales data into interactive visual dashboards that help compare actual revenue with forecast revenue and understand revenue performance across products, regions, customer segments, and time periods.

---

## Business Problem

Sara Boutique has several years of historical sales data but does not have an interactive visualization tool to analyse revenue trends and forecast future revenue.

Previously, future revenue was manually calculated using spreadsheets. This process is fixed, difficult to update, and may result in errors. Managers and finance teams also have limited ability to interact with the forecast, compare different situations, or easily identify differences between forecasted and actual revenue.

To address this problem, a **Tableau dashboard** was created using historical revenue data and Tableau's built-in forecasting capabilities.

The dashboard allows users to:

* Analyse historical revenue performance
* Understand monthly, quarterly, and yearly revenue patterns
* Compare revenue across categories
* Analyse regional revenue performance
* Analyse customer segment revenue
* Compare actual revenue with forecast revenue
* View the forecast range using confidence intervals
* Filter the analysis by category, region, segment, and year

---

## Example Business Scenario – Sara Boutique

Sara Boutique has four years of sales data but does not have a visualization tool for future revenue prediction.

The sales information is manually updated and future revenue is predicted using spreadsheets. This makes the process less interactive and difficult to update.

The Tableau dashboard uses historical sales patterns to project future revenue. It also displays a possible forecast range around the predicted revenue.

The boutique team can analyse categories such as **Sarees, Kurtis, and Women's Wear** and compare revenue performance across locations.

---

# Project Objectives

1. Analyse historical sales data to understand revenue performance.
2. Analyse monthly, quarterly, and yearly revenue patterns.
3. Analyse region-wise revenue performance.
4. Analyse customer segment revenue performance.
5. Analyse revenue trends over time.
6. Compare actual revenue with forecast revenue.

---

# Tools Used

### Python

Python was used for:

* Data loading
* Data cleaning
* Date parsing
* Feature engineering
* Column renaming
* Removing unnecessary columns
* Preparing the final dataset for Tableau

### Tableau

Tableau was used for:

* Data visualization
* Interactive filters
* Revenue trend analysis
* Revenue forecasting
* Actual vs forecast comparison
* Confidence interval visualization
* Revenue comparison analysis

---

# Python Data Preparation

The Python script performs the initial data preparation before the dataset is exported to Tableau.

### Main Steps

1. Load the revenue dataset.
2. Convert date fields into datetime format.
3. Calculate unit price with discount.
4. Calculate unit price without discount.
5. Calculate revenue amount without discount.
6. Rename the Sales column to Revenue With Discount.
7. Remove the unnecessary Order ID column.
8. Export the cleaned dataset for Tableau.

### Python Code

```python
import pandas as pd
import warnings
warnings.filterwarnings('ignore')

# 1. Load Dataset
df = pd.read_csv('Revenue Forecast.csv')

# 2. Convert Date Fields
df["Order Date"] = pd.to_datetime(df["Order Date"], errors="coerce")
df["Ship Date"] = pd.to_datetime(df["Ship Date"], errors="coerce")

# 3. Feature Engineering: Discount Calculations & Gross Revenue
df["Unit Price with Discount"] = (df["Sales"] / df["Quantity"]).round(2)

df["Unit Price without Discount"] = (
    df["Sales"] / (df["Quantity"] * (1 - df["Discount"]))
).round(2)

df["Revenue amount without Discount"] = (
    df["Unit Price without Discount"] * df["Quantity"]
).round(2)

# 4. Column Renaming
df.rename(
    columns={'Sales': 'Revenue With Discount'},
    inplace=True
)

# 5. Drop Unnecessary Identification Columns
df.drop(
    ["Order ID"],
    axis=1,
    inplace=True,
    errors="ignore"
)

# 6. Export Cleaned Dataset
df.to_csv(
    'RevenueForecastFinalDataset.csv',
    index=False
)
```

---

# Tableau Dashboard 1 – Revenue Forecast

## Dashboard Components

### KPI Cards

* **Total Actual Revenue:** ₹12.51M
* **Forecast Revenue:** ₹5M
* **Forecast Variance:** +7.8%

### Filters

The dashboard includes interactive filters for:

* Year of Order Date
* Category
* Region
* Segment

### Actual vs Forecast Revenue

A line chart displays historical actual revenue together with Tableau's exponential smoothing forecast.

The forecast includes a **95% confidence interval** and extends into 2026.

### Monthly Revenue Trend

A multi-line chart compares historical monthly revenue performance across different years.

### Revenue by Category

A horizontal bar chart compares total actual revenue across:

* Furniture
* Office Supplies
* Technology

### Revenue by Segment

A donut chart shows revenue distribution across:

* Consumer
* Corporate
* Home Office

### Revenue by Region

A vertical column chart compares revenue across:

* Central
* East
* South
* West

---

# Tableau Revenue Forecast Implementation

## 1. KPI Cards

Create worksheet views using:

```text
SUM([Revenue With Discount])
```

Apply custom number formatting:

```text
₹#,##0.00M
```

## 2. Actual vs Forecast

* Drag **Order Date** as continuous Month to Columns.
* Drag **Revenue With Discount** to Rows.
* Open the **Analytics Pane**.
* Select **Forecast**.
* Add the forecast to the view.
* Configure the forecast for 12 future periods.
* Use a 95% prediction interval.

## 3. Monthly Revenue Trend

* Place Order Date as Discrete Month on Columns.
* Place Revenue With Discount on Rows.
* Use Order Date Year for color coding.

## 4. Category Analysis

* Category → Rows
* Revenue With Discount → Columns
* Use a bar chart.

## 5. Segment Analysis

* Segment → Angle/Color
* Use a pie/donut chart.

## 6. Regional Analysis

* Region → Columns
* Revenue With Discount → Rows
* Use a bar/column chart.

---

# Tableau Dashboard 2 – Revenue Comparison

The second Tableau dashboard focuses on comparing revenue before and after discounts.

## KPI Cards

* **Revenue With Discount:** ₹12.51M
* **Revenue Without Discount:** ₹17.33M
* **Revenue Difference:** ₹4.83M
* **Discount Impact:** 27.84%

## Filters

Interactive filters are available for:

* Category
* Region
* Segment
* Year

## Dashboard Visualizations

### Revenue With vs Without Discount – Yearly

A dual-line trend compares:

* Revenue With Discount
* Revenue Without Discount

from 2023 to 2025.

### Revenue Comparison by Category

A grouped bar chart compares revenue with and without discounts across product categories.

### Revenue Comparison by Region

A grouped/stacked bar chart shows regional revenue and revenue loss caused by discounting.

### Revenue Comparison by Segment

A grouped bar chart compares net and gross revenue across customer segments.

---

# Tableau Calculated Fields

## Revenue Difference

```text
SUM([Revenue amount without Discount])
-
SUM([Revenue With Discount])
```

## Discount Impact %

```text
(
SUM([Revenue amount without Discount])
-
SUM([Revenue With Discount])
)
/
SUM([Revenue amount without Discount])
```

---

# Key Findings

## Revenue Forecast Dashboard

### Revenue Distribution

Revenue was relatively balanced across the three categories:

* Furniture: ₹4.17M
* Office Supplies: ₹4.18M
* Technology: ₹4.15M

### Customer Segment Performance

Revenue was distributed almost equally across:

* Corporate: 34%
* Consumer: 33%
* Home Office: 33%

### Regional Performance

The South region recorded the highest revenue at **₹3.25M**, followed by:

* West: ₹3.18M
* Central: ₹3.08M
* East: ₹3.00M

### Forecast Trend

The actual vs forecast visualization shows steady projected monthly performance of approximately **₹507K/month** into early 2026, with a **+7.8% projected variance**.

---

# Revenue Comparison Insights

### Discount Impact

Gross revenue potential was **₹17.33M**, while revenue after discount was **₹12.51M**.

This represents:

* Revenue Difference: **₹4.83M**
* Discount Impact: **27.84%**

### Category Discount Impact

Office Supplies experienced the highest absolute revenue loss:

* Gross Revenue: ₹5.84M
* Net Revenue: ₹4.18M

Furniture followed closely:

* Gross Revenue: ₹5.80M
* Net Revenue: ₹4.17M

### Customer Segment Discount Impact

Corporate customers had the highest gross discount allocation:

* Gross Revenue: ₹5.88M
* Net Revenue: ₹4.24M

This highlights the impact of discounting on corporate revenue.

---

# Project Workflow

```text
Historical Sales Data
        ↓
Python
        ↓
Data Cleaning & Preparation
        ↓
Feature Engineering
        ↓
Cleaned Dataset
        ↓
Tableau
        ↓
Interactive Visualizations
        ↓
Revenue Forecast Dashboard
        ↓
Revenue Comparison Dashboard
        ↓
Business Insights
```

---

# Project Outcome

The project transforms historical sales data into interactive Tableau dashboards for revenue analysis and forecasting.

Python was used to prepare the data, while Tableau was used to visualize historical performance, forecast future revenue, compare actual and forecast revenue, and analyse the impact of discounts.

The dashboards provide a visual way to understand revenue trends and support comparison of revenue performance across categories, regions, and customer segments.

