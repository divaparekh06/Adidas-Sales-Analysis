# Methodology

This document walks through the data preparation and DAX logic behind the dashboard, including a few mistakes caught and corrected along the way — kept intentionally, since debugging reasoning is as much a part of this project as the final result.

## 1. Data Cleaning (Power Query)

The source data arrived clean (no nulls, no duplicate rows), so cleaning focused on **shaping data correctly for analysis** rather than fixing errors:

- **Retailer ID**: changed from Whole Number → **Text**. It's an identifier, not a quantity — leaving it numeric risked accidental summation or incorrect numeric sorting in visuals.
- **Price per Unit, Total Sales, Operating Profit**: confirmed as Decimal — correct as-is.
- **Operating Margin (original column)**: removed entirely. This pre-computed column stored a per-row ratio (`profit ÷ sales` for that single transaction), which cannot be meaningfully summed or simply averaged across rows — a naive average would treat a $10 sale and a $10M sale as equally important. Replaced with a proper weighted DAX measure (see below).
- **Year** and **Month Number**: added via `Add Column → Date → Year` / `Month → Month`. Needed because the dataset spans two full years, and a Month Name column alone (e.g., "January") would incorrectly merge Jan 2020 and Jan 2021 into a single bucket on a time-series chart.

## 2. DAX Measures

### Operating Margin
```dax
Operating Margin = DIVIDE(SUM('Data Sales Adidas'[Operating Profit]), SUM('Data Sales Adidas'[Total Sales]), 0)
```
Built as a **measure**, not a calculated column, so it dynamically recalculates based on whatever's currently filtered (e.g., a single region via the slicer) rather than storing one static value per row. `DIVIDE()` is used instead of `/` to safely handle any filtered context where Total Sales could be zero.

**Verified:** $332,134,761.45 ÷ $899,902,125 = 36.9%, matching the value shown on the report.

### Sales per Unit
```dax
Sales per Unit = DIVIDE(SUM('Data Sales Adidas'[Total Sales]), SUM('Data Sales Adidas'[Units Sold]))
```
Same weighting problem as Operating Margin: a simple row-wise average of `Price per Unit` would treat every transaction as equally important regardless of volume. This measure instead computes a genuinely weighted average price by dividing total revenue by total units.

**Bugs caught during development:**
- First draft divided Units Sold by Total Sales (inverted) — caught by sanity-checking that the result should look like a plausible per-unit price (tens of dollars), not a tiny fraction.
- The measure was initially misnamed "Average Units Sold" despite calculating price — renamed to **Sales per Unit** to avoid misleading anyone reading the model later.

**Verified:** $899,902,125 ÷ 2,478,861 units = $363.03, matching the card value.

## 3. Chart-Specific Fixes

### Area Chart — Sales by Month
**Bug:** initial version sorted the X-axis by `Total Sales` value (descending) instead of by date, producing a smooth-looking but meaningless curve — July 2021 (highest sales) appeared first, December 2020 (lowest) appeared last, with no real chronological meaning.

**Fix:** changed the visual's sort field to `Year` → `Month Number`, ascending — producing a genuine chronological trend line with real seasonal dips and peaks.

| Before (sorted by value) | After (sorted by date) |
|---|---|
| ![Unsorted](screenshots/01_area_chart_unsorted_bug.png) | ![Sorted](screenshots/02_area_chart_sorted_correct.png) |

### Map — Sales by State
**Bug:** `Total Sales` was placed in the **Legend** field well, which is meant for categorical data. Since Total Sales has hundreds of unique values, this produced dozens of individual color swatches instead of a gradient.

**Fix:** moved `Total Sales` to the **Color Saturation** field well (the correct field for continuous numeric values), producing a proper light-to-dark gradient.

| Before (Legend, wrong) | After (Color Saturation, correct) |
|---|---|
| ![Legend bug](screenshots/03_filled_map_legend_bug.png) | ![Correct map](screenshots/04_map_by_state_correct.png) |

### Bar Chart — Sales by Retailer
**Bug:** the Values field well accidentally contained `State` with a Count aggregation, producing "Count of State by Retailer" instead of Total Sales.

**Fix:** replaced with `Total Sales`, explicitly confirmed as Sum aggregation.

## 4. Interactivity

- **Region Slicer**: standard categorical slicer, cross-filters all 5 visuals.
- **Invoice Date Slicer**: set to "Between" (range) style rather than a dropdown/list — chosen deliberately, since a 2-year daily dataset would produce an impractically long checkbox list in list/dropdown format.
