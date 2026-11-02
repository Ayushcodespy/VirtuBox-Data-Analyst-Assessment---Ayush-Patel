# Data Cleaning Log

## 1. Missing Values Analysis

The two worksheets were combined into a single DataFrame for analysis.

The combined dataset contains **1,067,371 rows and 8 columns**.

Missing values were examined using:
- Missing value counts
- Missing value percentages

No missing values were removed or imputed during the initial analysis. The handling strategy was determined after investigating the business relevance of the affected records.

## 2. Missing Values Investigation

### Findings

| Column | Missing Values | Missing Percentage |
|---|---:|---:|
| Customer ID | 243,007 | 22.77% |
| Description | 4,382 | 0.41% |

All remaining columns have no missing values.

### Initial Business Interpretation

Missing Customer ID values may represent transactions where the customer could not be identified. These records may still be useful for transaction-level revenue analysis but may not be suitable for customer-level retention analysis.

Missing product descriptions were investigated using StockCode to determine whether descriptions could be reliably recovered from other records.

### Initial Decision

No rows were deleted, and no missing values were imputed at this stage.

The final treatment was based on the investigation results.

## 3. Product Description Recovery

### Investigation

A total of 4,382 records had missing product descriptions.

The investigation identified:
- 2,451 unique StockCodes among records with missing descriptions.
- 2,096 of these StockCodes had descriptions available in other records.
- 1,232 StockCodes in the dataset had multiple distinct descriptions.

### Cleaning Method

Descriptions were recovered only when a StockCode had exactly one unique non-missing description in the dataset.

StockCodes with multiple distinct descriptions were not used for automatic mapping because the mapping could be ambiguous.

### Results

- Missing descriptions before recovery: 4,382
- Missing descriptions recovered: 3,062
- Missing descriptions remaining: 1,320

No rows were deleted as part of this operation.

## 4. Missing Customer ID Investigation

A total of 243,007 records had missing Customer ID values in the combined dataset.

### Findings

- Positive quantity records: 238,801
- Negative quantity records: 4,206
- Zero quantity records: 0

### Decision

Records with missing Customer ID were retained for transaction-level sales analysis.

They will be excluded from customer-specific retention analysis where customer identification is required.

Missing Customer IDs were not replaced with a common placeholder because doing so could incorrectly group unrelated transactions under a single customer.

## 5. Duplicate Removal

Exact duplicate rows were identified and removed from the working dataset.

### Results

- Rows before duplicate removal: 1,067,371
- Exact duplicate rows removed: 34,335
- Rows after duplicate removal: 1,033,036

Only exact duplicate rows were removed. Records with the same invoice or product but different values were not automatically treated as duplicates.

## 6. Data Type Validation

The data types of all columns were reviewed.

### Findings and Actions

- `Invoice`: Retained as an identifier.
- `StockCode`: Retained as an identifier.
- `Description`: Retained as text.
- `Quantity`: Retained as integer.
- `InvoiceDate`: Already in datetime format.
- `Price`: Retained as floating-point numeric data.
- `Customer ID`: Converted from `float64` to pandas nullable integer type `Int64`.
- `Country`: Retained as text.

No fractional Customer ID values were found before conversion.

The nullable integer type preserves missing Customer IDs without replacing them with artificial values.

## 7. Quantity and Price Validation

### Findings

| Check | Count |
|---|---:|
| Negative Quantity | 22,496 |
| Zero Quantity | 0 |
| Negative Price | 5 |
| Zero Price | 6,014 |

### Negative Quantity Investigation

Negative quantities may represent returns or cancellations. These records were retained because they are relevant to sales and returns analysis.

### Negative Price Investigation

The five negative-price records were identified as `Adjust bad debt` entries with StockCode `B`.

These appear to represent accounting adjustments rather than ordinary product sales.

### Decision

- Retained negative quantities because they may represent returns or cancellations.
- Retained zero-price records because they may represent free or promotional items.
- Retained negative-price accounting adjustments in the cleaned dataset.
- Accounting adjustment records will be excluded from ordinary product sales analysis where appropriate.

### Reason

The dataset contains both product transactions and accounting-related records. Retaining the original records preserves information, while separate analytical filters prevent accounting adjustments from distorting product sales metrics.

## 8. Revenue Calculation

A new `Revenue` column was created using the following formula:

**Revenue = Quantity × Price**

### Results

- Total records: 1,033,036
- Average revenue per record: approximately 18.25
- Median revenue per record: 9.92
- Minimum revenue: -168,469.60
- Maximum revenue: 168,469.60

### Decision

Revenue was calculated for all records in the cleaned dataset, including negative quantities, zero-price transactions, and accounting adjustments.

These records were retained at this stage. Appropriate analytical filters will be applied when preparing datasets for specific business questions.

## 9. Final Data Quality Summary

The data cleaning and preparation process was completed on the combined Online Retail II dataset.

### Final Dataset Overview

| Metric | Result |
|---|---:|
| Original rows | 1,067,371 |
| Rows after duplicate removal | 1,033,036 |
| Exact duplicates removed | 34,335 |
| Final columns | 9 |
| Missing Description values | 1,320 |
| Missing Customer ID values | 235,151 |
| Negative Quantity records | 22,496 |
| Zero Price records | 6,014 |
| Negative Price records | 5 |

### Final Preparation

The following operations were completed:

- Combined the two worksheets into one DataFrame.
- Investigated missing values.
- Recovered product descriptions where StockCode mapping was unambiguous.
- Removed exact duplicate rows.
- Validated and converted data types.
- Investigated negative and zero quantity/price records.
- Created a Revenue column using Quantity × Price.

### Conclusion

The dataset is prepared for exploratory data analysis. Missing customer identifiers, returns, zero-price transactions, and accounting adjustments have been retained where appropriate.

Analytical filters will be applied according to the business questions being investigated.
