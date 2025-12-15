# Amazon International Apparel Sales Data Cleaning Challenge

This is a Project I found on twitter. The challenge is [Amazon International Apparel Sales](https://docs.google.com/document/d/1dbqTWqOOhXfrLKP9fwqbyZGX0R629uhc4dhEcHNBkwI/edit?usp=sharing). I am going to try as much as I can to draw insight so be with me.

### 📋 Executive Summary

**Business Health:** The business is profitable but follows a `"Long Tail"` model, where revenue is highly fragmented across many products rather than relying on a few `"Hero"` items. This suggests high portfolio stability but higher inventory management complexity.

**Critical Risk:** We face `high Customer Concentration risk`, where the `top 8 customers control 33.83% of total revenue`, leaving us vulnerable due to heavy reliance on a small number of key accounts.

**Key Opportunity:** Operational efficiency can be improved by addressing underperforming inventory, which accounts for 87% of our catalog (Dead Stock).

### Data Cleaning & Preparation

**Objective**: Prepare raw sales data for analysis by resolving formatting inconsistencies.

1. First we will open the [CSV File](https://drive.google.com/file/d/1miw_rlKK6JghPuFlTaZCzFklNvDbprBr/view?usp=sharing) with excel
2. Find all the blanks rows

This is what we found

| Date | Months | CUSTOMER | Style | SKU | Size | PCS | Rate | GROSS AMT |
| ---- | ------ | -------- | ----- | --- | ---- | --- | ---- | --------- |
| 1 | 26 | 1041 | 1041 | 2475 | 1041 | 1041 | 1041 | 1041 |


Since the number 1040 appears across `CUSTOMER`, `Style`, `PCS`, `RATE`, and `GROSS AMT`, it strongly suggests these rows are missing all key transaction data.

If a row has no customer, no product style, and no price information, is it useful for our analysis?

We can't analyze a sale if we don't know who bought it or what they bought.

Delete 1041 rows using `CUSTOMER` column Filter Method. Clear the filter we get `36,392` actual data rows

**Initial Challenge**: The raw CSV file contained varying date formats (e.g., 6/5/21 vs. 2021-06-05).

- The `MySQL Bouncer` Issue: When importing to MySQL, the strict date requirements caused ~85% of the data to be rejected (dropping from ~18,000 rows to ~2,700).

- **Decision**: Switched to Excel for data cleaning to leverage its flexibility.

- **Outcome**: Successfully retained 100% of the data (18,634 valid rows) with cleaned Dates and Numeric columns.


**Find [Cleaned Data](https://docs.google.com/spreadsheets/d/1g4wPbzRGyjtm78mN4IlQEtxuio5EhDVkl_8GiAzaqAI/edit?usp=sharing)**

## Sales & Financial Performance

### 1. Business Analysis: Revenue Drivers

**Question**: Which specific Styles or SKUs account for the top 80% of total gross revenue? (Testing the Pareto Principle/80-20 Rule)

**Methodology**:

- Created a Pivot Table grouping by Style.

- Sorted by `GROSS AMT` (Descending).

- Calculated `% of Grand Total` and `Running Total`.

*Key Findings*:

**Top Performer**: The #1 best-selling style is `SET268`, contributing `1.18%` of total revenue.

**Market Structure**:

**Total Styles**: `1,035`

Styles required to reach 80% Revenue: `425`

**Insight**: The business does not follow a strict 80/20 rule.

`~41%` of products are required to generate 80% of revenue.

**Conclusion**: This indicates a `"Long Tail"` business model. Revenue is highly fragmented across many products rather than relying on a few `"Hero"` items. This suggests high portfolio stability but higher inventory management complexity.

### 2. Profitability Analysis

**Question**: Which Styles have the highest average unit price (RATE)?

**Methodology**:

Modified Pivot Table to show `Average of RATE` instead of Sum.

Sorted Average of RATE from Largest to Smallest.

**Key Findings**:

1. Most Expensive Style: `SET433`

2. Average Price: `687.50`

**Insight**: This item represents the "Premium" tier of the catalog, significantly higher than the average item.

### 3. Seasonal Trends

**Question**: How does monthly GROSS AMT change over the year?

**Methodology**:

Created a Pivot Table with `Month` in `Rows` and `Sum of GROSS AMT` in `Values`.

Analyzed the revenue distribution across months.

**Key Findings**:

1. Peak Season: October appears to be the highest revenue month.

2. Data Quality Note: The months are currently sorting alphabetically (e.g., April, August...) rather than chronologically.

**Insight**: This suggests the "Month" column is being treated as Text rather than Date objects. For a final report, a custom sort order (Jan-Dec) would be required to accurately visualize the seasonal curve.

### 5. Sales Velocity

**Question**: What is the sales volume for the best-selling SKUs?

**Methodology**:

Created a Pivot Table with `SKU` in `Rows` and `Sum of Qty` in `Values`.

Sorted by `Quantity` (Largest to Smallest).

**Key Findings**:

1. Top Selling SKU (Volume): CMB5-JNE3853

2. Total Units Sold: 70

**Insight**: While SET268 generated the most revenue (due to higher price), CMB5-JNE3853 moved the most units.

**Note**: To calculate true "velocity" (sales per month), divide this total (70) by the number of months in the dataset.

# 🛒 Customer & Market Analysis

### 6. Top Tier Customers
**Question:** Who are the top 10 CUSTOMERS based on total revenue?
* **Methodology:** Pivot Table (`Customer` in Rows, `Sum of GROSS AMT` in Values).
* **Top Customer:** `MULBERRIES BOUTIQUE
* **Revenue Contribution:** `2,094,071`
* **Characteristics of Size:** `XXL`
* **Insight:** This customer is a significant outlier, likely representing a B2B/Wholesale account given the high volume compared to average transaction sizes.

### 7. Customer Concentration
**Question:** What percentage of total revenue comes from the top 5% of customers?
* **Methodology:** Pivot Table (`Customer` in Rows, `Sum of GROSS AMT` in Values).
     - Identified `total customer count` (150) and `calculated the top 5% threshold` (8 customers).
     - Summed the GROSS AMT for these top `8` and `divided by the Grand Total`.
* **Key Findings:**
  - **Concentration:** The top 8 customers contribute `33.83% of total revenue`.
  - **Risk Assessment:** High. The business is heavily reliant on a small number of key accounts (likely wholesale).
  - **Recommendation:** Prioritize relationship management for these 8 VIPs while trying to diversify the customer base to reduce risk.

# 📦 Product & Inventory Management

### 8. Dead Stock / Underperformers
**Question:** Which Styles have sold less than 50 PCS?
* **Methodology:**
     - Created a Pivot Table with `Style` in Rows and `Sum of Qty` in Values.
     - Filtered  to find items with `total quantity < 50`.
* **Key Findings:**
  - **Dead Stock Count: `908 Styles`**
  - **Impact:** Approximately `87%` of the entire product catalog has `sold fewer than 50 units` in the last year.
  - **Action Item:** This indicates a massive opportunity to clear out old inventory and focus future production on the ~12% of styles that are actually moving.

### 9. Popular Size Distribution
**Question:** What is the breakdown of sales by Size?
* **Methodology:**
     - Created a Pivot Table with `Size` in Rows and `PCS` in Values.
     - Sorted by `PCS` (Largest to Smallest).
* **Key Findings:**
  - **Top Selling Size: Large (L)**
  - **Volume:** 4,754 units
  - **Insight:** Inventory planning should prioritize stocking `"Large"` sizes as they move the fastest,  followed closely by `Medium` & `XL`.

### 9. Popular Size Distribution
**Question:** In the SKU column, are there any Styles that map to more than one distinct "Middle Code" (e.g., uses both -KR- and -SKD-) that should be standardized?
* **Methodology:**
     - Created a Pivot Table hierarchy with `Style` -> `SKU`.
     - Scanned for Styles containing multiple distinct `SKU` patterns (e.g., `-KR-`, `-SKD-`, `-JNE-`).
* **Key Findings:**
  - **Observation:** Yes, **many styles** map to multiple distinct middle codes.
  - **Impact:** This causes **Data Fragmentation**. The same physical product is being tracked as separate entities, splitting the sales and inventory data.
  - **Recommendation:** Future data cleaning pipelines should standardize these codes (e.g., strip the middle code or map them to a master SKU) to ensure accurate total volume reporting.

### 10. Pricing Consistency
**Question:** Is there significant variance in the RATE charged for the same SKU to different CUSTOMERS?
* **Methodology:**
     - Created a Pivot Table hierarchy: `SKU` $\rightarrow$ `RATE` $\rightarrow$ `CUSTOMER`.
     - Analyzed specific `SKU`s to see if multiple price points existed for the same item.
* **Key Findings:**
  - **Status: Inconsistent Pricing Detected:**
  - **Evidence:** `SKU` `BTM038-PP-L` was sold at multiple rates (i.e., 393.75, 428.00, and 438.75) to different customers within the same period.
  - **Insight:** The business likely employs **variable pricing** strategies (discounts, negotiations, or tiers).

* In the business world, this is often intentional and is called **Price Tiering** or **Dynamic Pricing**.

🏷️

* **Volume Discounts:** Maybe Mohana bought 50 units, while Visha only bought 1?

* **Customer Loyalty:** Older customers often keep "legacy rates" while new ones pay the current market price.

* **Negotiation:** B2B clients (like "Amani Concept") often negotiate specific contracts that differ from standard rates.


```python

```
