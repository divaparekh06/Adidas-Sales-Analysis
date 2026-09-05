# Dataset

**Note:** The raw dataset file is intentionally **not committed** to this repo. Source data typically shouldn't live in a code/analysis repo — it bloats repo size, complicates version control, and can raise licensing questions. Instead, it's described here and can be sourced independently or requested.

## Overview

- **Rows:** 9,648 individual sales transactions
- **Date range:** January 2020 – December 2021
- **Grain:** One row per transaction (retailer, state, product, date)

## Columns

| Column | Type | Description |
|---|---|---|
| Retailer | Text | Retail partner (Foot Locker, Walmart, Sports Direct, West Gear, Kohl's, Amazon) |
| Retailer ID | Text | Internal identifier — stored as text, not a numeric measure |
| Invoice Date | Date | Transaction date |
| Region | Text | One of 5 US regions |
| State | Text | US state, used for geographic mapping |
| City | Text | City of sale |
| Product | Text | One of 6 product categories |
| Price per Unit | Decimal | Per-unit price for that transaction |
| Units Sold | Whole number | Units sold in that transaction |
| Total Sales | Decimal | Revenue for that transaction |
| Operating Profit | Decimal | Profit for that transaction |
| Operating Margin | Decimal | *(Original column — dropped during Power Query cleaning; recalculated correctly as a DAX measure instead. See [METHODOLOGY.md](../docs/METHODOLOGY.md).)* |
| Sales Method | Text | In-store, Outlet, or Online |

## Data Quality

Verified during cleaning: no null values, no duplicate rows, and all data types were appropriate or corrected in Power Query (see methodology doc for the specific fixes made).
