# Adidas US Sales Dashboard (Power BI)

An interactive Power BI dashboard analyzing 2 years (2020–2021) of Adidas US sales transactions — built to practice AI-assisted, human-verified analytics: using AI to accelerate DAX/formula generation and Power Query cleaning, while manually validating every output against the raw data before trusting it.

## Dashboard Preview

![Sales by Month](docs/screenshots/02_area_chart_sorted_correct.png)
![Sales by State](docs/screenshots/04_map_by_state_correct.png)
![Sales by Product and Retailer](docs/screenshots/05_bar_charts_product_retailer.png)

## Features

| KPI Card | Description |
|---|---|
| **Total Sales** | Sum of all transaction revenue: **$899.9M** |
| **Operating Profit** | Sum of profit across all transactions: **$332.1M** |
| **Units Sold** | Total units moved: **2.48M** |
| **Sales per Unit** | Weighted average price per unit (`Total Sales ÷ Units Sold`, not a row-wise average): **$363.03** |

## Visualizations

1. **Area Chart — Total Sales by Month** — monthly trend across 2020–2021, sorted chronologically (Year → Month Number).
2. **Map — Total Sales by State** — geographic view of sales intensity across all 50 states, using color saturation for a continuous gradient.
3. **Bar Chart — Total Sales by Product** — ranks 6 product categories by revenue.
4. **Bar Chart — Total Sales by Retailer** — ranks 6 retail partners by revenue.
5. **Donut Chart — Total Sales by Region** — revenue share across 5 US regions.

## Interactivity

- **Region Slicer** — filters all visuals by region.
- **Invoice Date Slicer** (Between/range style) — filters all visuals across the 2-year date range.

## Data Source

Adidas US Sales dataset — 9,648 transaction-level rows covering Jan 2020–Dec 2021, across 5 regions, 50 states, 6 retailers, 6 products, and 3 sales methods. See [`data/README.md`](data/README.md) for details and column definitions.

## Tools & Approach

- **Power Query** for data shaping: data type verification, removal of a flawed pre-computed `Operating Margin` column, and calculated `Year`/`Month Number` columns for correct chronological charting.
- **DAX measures** for correct aggregation logic (see [`docs/METHODOLOGY.md`](docs/METHODOLOGY.md) for the full reasoning behind each measure, including two subtle weighting bugs caught and fixed during development).
- **AI-assisted, human-verified workflow**: formulas and logic were reasoned through interactively, then checked against manually recomputed values from the raw dataset before being trusted.

## How to Open

1. Requires Power BI Desktop (May 2024 or later recommended).
2. Download `Adidas_Sales_Dashboard.pbix` from this repo.
3. Open in Power BI Desktop — no additional dependencies required.
4. Use the Region and Invoice Date slicers to filter; hover over any visual for tooltips.

## Key Insights

- Sales show clear seasonality, dipping in early-to-mid 2020 and climbing through most of 2021, peaking around July–August before a brief October dip.
- California and New York are the strongest state-level markets by total sales.
- Men's Street Footwear is the top-selling product category; Women's Athletic Footwear the lowest of the six.
- Foot Locker is the largest retail partner by volume, Walmart the smallest.

## Future Enhancements

- Year/Quarter-level time filter separate from the continuous date slicer.
- Additional KPIs: Profit Margin by Region, repeat-retailer trend analysis.

## Repo Structure

```
adidas-sales-dashboard/
├── README.md
├── Adidas_Sales_Dashboard.pbix
├── data/
│   └── README.md
└── docs/
    ├── METHODOLOGY.md
    └── screenshots/
```
