# Amazon International Apparel Sales Data Cleaning Challenge 🛒

## 📌 Project Overview
This project focuses on cleaning and analyzing a messy dataset of international apparel sales to uncover actionable business insights. The raw data contained significant inconsistencies, requiring a strategic approach to data preparation before analysis could begin.

**Original Challenge:** [Amazon International Apparel Sales Data Cleaning Challenge](https://docs.google.com/document/d/1dbqTWqOOhXfrLKP9fwqbyZGX0R629uhc4dhEcHNBkwI/edit?usp=sharing) by Joseph Edet.

## 📋 Executive Summary
* **Business Health:** The business is profitable but follows a "Long Tail" model, where revenue is highly fragmented across many products rather than relying on a few "Hero" items. This suggests high portfolio stability but higher inventory management complexity.
* **Critical Risk:** We face high **Customer Concentration** risk, where the top 8 customers control **33.83%** of total revenue, leaving us vulnerable due to heavy reliance on a small number of key accounts.
* **Key Opportunity:** Operational efficiency can be improved by addressing **underperforming inventory**, which accounts for **87%** of our catalog (Dead Stock).

## 🛠️ Data Cleaning Strategy
To guarantee data **completeness**, our process began by performing a rigorous audit in Excel, identifying and removing 1,040 rows that contained null values in critical fields such as `Customer`, `Style`, `Quantity`, `Rate`, and `Gross Amount`. These incomplete records were excluded to prevent skewed analysis. Following this, we attempted to migrate the dataset to MySQL Workbench. However, the database’s strict **validity** constraints rejected over 90% of the records (dropping from 37,432 to 2,766 rows) due to schema violations in the raw data.

To preserve the integrity of the full dataset, we pivoted back to Excel for flexible sanitation. During this phase, we uncovered a major **consistency** issue: the 'Date' column was contaminated with customer names. This anomaly led to the discovery of duplicate headers and interleaved stock inventory data. We removed these rows to ensure the **accuracy** and relevance of the final dataset, strictly limiting it to sales transactions. Finally, we standardized all date entries to a unified format, resulting in a clean, high-quality dataset ready for analysis.

## 📊 Key Findings

### 1. Revenue Drivers (The Long Tail)
* **Observation:** The top 80% of revenue is driven by ~41% of the product catalog (425 styles), rather than the typical "20%" seen in Pareto analyses.
* **Insight:** The business relies on a broad catalog rather than a few hit products.

### 2. Customer Concentration (The Whales)
* **Top Customer:** `MULBERRIES BOUTIQUE` purchased over **2 million** in goods.
* **Risk:** The top 8 customers alone contribute **33.83%** of total revenue.
* **Recommendation:** Prioritize relationship management for these VIPs, as losing even one would be financially damaging.

### 3. Inventory Health (Dead Stock)
* **Issue:** **908 Styles** (87% of the catalog) have sold fewer than 50 units in the last 12 months.
* **Action Item:** A massive opportunity exists to liquidate slow-moving stock and free up capital for high-performing styles.

## 💻 Tool Used
* **Microsoft Excel:** Data Cleaning, Date Standardization, Anomaly Detection.

---
*Author: aTfure*
