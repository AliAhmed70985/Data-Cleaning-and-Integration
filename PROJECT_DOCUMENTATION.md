# Project Documentation
# Data Cleaning and Integration Using Python

## 1. Introduction

This project was developed to clean and integrate multiple datasets related to sales transactions, customers, products, and daily summaries.

The project uses Python, Pandas, and NumPy to identify and handle common data quality issues and prepare structured datasets for further analysis.

The main objective is to transform raw datasets into consistent and reliable data while preserving the original information.

## 2. Tools and Libraries

The following tools and libraries were used:

- **Python:** Main programming language.
- **Pandas:** Used for loading, cleaning, transforming, merging, and exporting datasets.
- **NumPy:** Used for numerical operations and conditional data handling.
- **Google Colab:** Cloud-based environment used to execute the notebook.
- **Zipfile and OS:** Used to extract and manage dataset files.

## 3. Original Datasets

Five CSV files were provided.

### 3.1 Orders Dataset

Contains historical sales transaction information.

Main columns include:
- Order_ID
- Order_Date
- Ship_Date
- Customer_ID
- Product_ID
- Quantity
- Unit_Price
- Discount
- Shipping_Cost
- Order_Status
- Payment_Method

Original shape: 1,020 rows and 11 columns.

### 3.2 New Daily Batch Dataset

Contains additional order records that need to be cleaned and integrated with the historical orders.

It has the same 11 columns as the Orders dataset.

Original shape: 333 rows and 11 columns.

### 3.3 Customers Master Dataset

Contains customer information, including:
- Customer_ID
- Customer_Name
- Region
- Customer_Segment

Original shape: 150 rows and 4 columns.

### 3.4 Products Master Dataset

Contains product and inventory information, including:
- Product_ID
- Product_Name
- Category
- Unit_Cost
- Current_Stock
- Reorder_Level

Original shape: 30 rows and 6 columns.

### 3.5 Previous Days Summary Dataset

Contains daily summary and data quality metrics, including:
- Date
- Record_Count
- Total_Orders
- Total_Sales
- Valid_Records_Rate
- Rejected_Records
- Missing_Rate
- Duplicate_Rate

Original shape: 30 rows and 8 columns.

## 4. Data Loading and Initial Exploration

The first stage involved importing the required libraries and extracting the provided ZIP archive.

The five CSV files were loaded into separate Pandas DataFrames.

Initial exploration was performed using:

- `shape` to inspect dataset dimensions.
- `info()` to examine data types and non-null values.
- `head()` to preview records.
- `isnull().sum()` to identify missing values.
- `duplicated().sum()` to detect exact duplicate rows.
- `value_counts()` to inspect categorical values.

This stage helped identify the main data quality issues before cleaning.

## 5. Data Quality Assessment

The initial inspection identified several issues.

### 5.1 Missing Values

The following missing values were identified:

**Orders:**
- Ship_Date: 215
- Customer_ID: 3
- Unit_Price: 1

**New Daily Batch:**
- Ship_Date: 14
- Customer_ID: 4
- Shipping_Cost: 9

The Customers, Products, and Previous Days Summary datasets had no missing values in the initial assessment.

### 5.2 Duplicate Records

Exact duplicate rows were identified in the two transaction datasets:

- Orders: 2 duplicate rows.
- New Daily Batch: 4 duplicate rows.

No exact duplicate rows were found in the other three datasets.

### 5.3 Inconsistent Categorical Values

Some categorical columns contained inconsistent capitalization and unnecessary spaces.

Examples included:
- Different capitalization in Order_Status.
- Different capitalization in Payment_Method.
- Inconsistent formatting in customer regions.
- Inconsistent formatting in product categories.

These values were standardized during the cleaning process.

## 6. Cleaning the Orders Dataset

A separate copy named `orders_clean` was created to preserve the original dataset.

The following operations were performed:

### 6.1 Remove Duplicates

Exact duplicate rows were removed using:

`drop_duplicates()`

The dataset was reduced from 1,020 to 1,018 records.

### 6.2 Standardize Text

The Order_Status and Payment_Method columns were standardized using:

- `str.strip()` to remove unnecessary spaces.
- `str.title()` to apply consistent capitalization.

### 6.3 Convert Data Types

The Order_Date and Ship_Date columns were converted into datetime format using `pd.to_datetime()`.

The Unit_Price column was converted into a numeric data type using `pd.to_numeric()`.

Invalid conversion values were treated as missing values.

### 6.4 Handle Invalid Numerical Values

Negative quantities were treated as invalid and replaced with missing values.

Discount values outside the range of 0 to 1 were also treated as invalid.

The Quantity column was converted into Pandas nullable integer format.

### 6.5 Validate Shipping Dates

Shipping dates earlier than their corresponding order dates were identified and replaced with missing values.

Missing shipping dates were preserved because some orders may not have been shipped.

## 7. Cleaning the New Daily Batch Dataset

A separate working copy named `new_batch_clean` was created.

### 7.1 Remove Duplicates

Four exact duplicate rows were removed.

The dataset was reduced from 333 to 329 records.

### 7.2 Clean Quantity

Some Quantity values were stored as text and included the suffix `pcs`.

The suffix was removed, and the values were converted into numeric format.

Negative quantities were treated as missing values.

### 7.3 Standardize Discount

The Discount column contained both decimal values and percentage strings.

Examples:
- `0.10`
- `10%`

Percentage values were divided by 100 to convert them into decimal format.

Values outside the valid range of 0 to 1 were treated as missing.

### 7.4 Standardize Text and Dates

Order_Status and Payment_Method were standardized using `str.strip()` and `str.title()`.

Order_Date and Ship_Date were converted into datetime format.

Shipping dates earlier than order dates were identified and treated as missing.

## 8. Cleaning Customers and Products

### 8.1 Customers Dataset

A working copy named `customers_clean` was created.

The Region column was standardized by removing extra spaces and applying consistent capitalization.

Customer_ID values were checked for duplicates.

The dataset contained 150 records.

### 8.2 Products Dataset

A working copy named `products_clean` was created.

The Category column was standardized.

Product_ID values were checked for duplicates.

The following numerical columns were converted into numeric format:

- Unit_Cost
- Current_Stock
- Reorder_Level

Negative values were treated as invalid and replaced with missing values.

Zero stock values were preserved because zero inventory is a valid business condition.

The dataset contained 30 records.

## 9. Cleaning Previous Days Summary

A separate copy named `summary_clean` was created.

The Date column was converted into datetime format.

The following columns were converted into numeric data types:

- Record_Count
- Total_Orders
- Total_Sales
- Valid_Records_Rate
- Rejected_Records
- Missing_Rate
- Duplicate_Rate

Exact duplicates, duplicate dates, and missing values were checked.

The rate columns were also validated to ensure their values were within the expected decimal range from 0 to 1.

## 10. Data Integration

After cleaning the individual datasets, the transaction datasets were prepared for integration.

### 10.1 Standardize Identifiers

The following identifier columns were standardized as string values:

- Order_ID
- Customer_ID
- Product_ID

Extra spaces were removed to ensure consistent matching.

Customer_ID and Product_ID formats were also standardized in their corresponding master datasets.

### 10.2 Combine Orders

The cleaned historical orders and the cleaned new daily batch were combined using `pd.concat()`.

The resulting dataset was named `all_orders`.

Final combined shape:

**1,347 rows and 11 columns.**

### 10.3 Validate Customer IDs

Customer IDs in the combined dataset were compared with the identifiers in `customers_clean`.

The validation identified:
- 7 records with missing Customer IDs.
- 4 records with unmatched Customer IDs.

The unmatched values were:
- C5555
- C6666

### 10.4 Validate Product IDs

Product IDs in the combined dataset were compared with the identifiers in `products_clean`.

The validation identified:
- 0 records with missing Product IDs.
- 6 records with unmatched Product IDs.

The unmatched values were:
- PX01
- PX02
- P555
- P666

## 11. Merge with Master Datasets

The combined orders dataset was enriched by merging it with the Customers and Products master datasets.

Two left joins were performed:

1. Merge with `customers_clean` using Customer_ID.
2. Merge with `products_clean` using Product_ID.

The resulting dataset was named `orders_enriched`.

A left join was used to preserve all original transaction records, including records with missing or unmatched identifiers.

The merge produced:

**1,347 rows and 21 columns.**

The number of rows remained unchanged, confirming that the merge preserved all transaction records.

## 12. Identify Invalid References

Two additional columns were created:

- `Customer_ID_Status`
- `Product_ID_Status`

Each column classifies the corresponding identifier as:

- Valid
- Missing
- Unmatched

The validation results were:

| Customer ID Status | Product ID Status | Records |
|---|---|---:|
| Valid | Valid | 1,330 |
| Missing | Valid | 7 |
| Unmatched | Valid | 4 |
| Valid | Unmatched | 6 |

A total of 17 records contained missing or unmatched customer or product references.

## 13. Create the Final Valid Dataset

A separate dataset named `valid_orders` was created.

Only records with both valid Customer IDs and valid Product IDs were retained.

The original cleaned datasets and the enriched dataset were not modified or deleted.

Final result:

- Original enriched records: 1,347
- Final valid records: 1,330
- Excluded records: 17

The 17 excluded records were retained in the combined and enriched datasets for reference.

## 14. Final Validation

The final dataset was checked to ensure that:

- No Customer IDs were missing.
- No Product IDs were missing.
- No Customer IDs were unmatched.
- No Product IDs were unmatched.
- No exact duplicate rows remained.

Final validation results:

| Validation Check | Result |
|---|---:|
| Final Dataset Rows | 1,330 |
| Final Dataset Columns | 21 |
| Missing Customer IDs | 0 |
| Missing Product IDs | 0 |
| Invalid Customer IDs | 0 |
| Invalid Product IDs | 0 |
| Duplicate Rows | 0 |

**Important:** These checks confirm the validity of customer and product references and the absence of duplicate rows. They do not mean that every column in the final dataset is completely free of missing values. Some missing dates and numerical values were preserved for further investigation or analysis.

## 15. Exporting the Results

All final datasets were exported as CSV files into a folder named `cleaned_data`.

The exported files include:

1. `cleaned_orders.csv`
2. `cleaned_new_daily_batch.csv`
3. `cleaned_customers.csv`
4. `cleaned_products.csv`
5. `cleaned_previous_days_summary.csv`
6. `combined_orders.csv`
7. `enriched_orders.csv`
8. `final_valid_orders.csv`
9. `final_quality_report.csv`

All nine files were compressed into:

`cleaned_data_final.zip`

## 16. Conclusion

The project successfully cleaned and integrated five datasets using Python and Pandas.

The workflow addressed duplicate records, inconsistent text formatting, invalid numerical values, date validation, and identifier matching.

The historical orders and new daily batch were combined and enriched with customer and product information.

A final dataset containing 1,330 records with valid customer and product references was prepared and exported for further analysis.

The original cleaned and enriched datasets were also preserved to maintain data traceability and allow further investigation of excluded records.