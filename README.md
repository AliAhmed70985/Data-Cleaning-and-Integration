# Data Cleaning and Integration Project

## Project Overview

This project focuses on cleaning, validating, and integrating multiple sales-related datasets using Python and Pandas.

The main objective is to prepare reliable and structured datasets by identifying data quality issues, handling duplicates, standardizing inconsistent values, validating identifiers, and combining different datasets into a unified dataset.

The project was implemented using **Python in Google Colab**.

## Technologies Used

- Python
- Pandas
- NumPy
- Google Colab
- CSV Files

## Dataset Description

The project contains five original datasets:

| Dataset | Description | Original Records |
|---|---|---:|
| orders.csv | Historical order transactions | 1,020 |
| new_daily_batch.csv | New daily order records | 333 |
| customers_master.csv | Customer information | 150 |
| products_master.csv | Product and inventory information | 30 |
| previous_days_summary.csv | Daily data quality and sales summary | 30 |

## Project Workflow

### 1. Data Loading and Exploration
- Imported the required Python libraries.
- Extracted the original dataset files.
- Loaded all five CSV files using Pandas.
- Inspected dataset dimensions, columns, data types, and sample records.

### 2. Data Quality Assessment
- Identified missing values.
- Detected exact duplicate records.
- Inspected inconsistent categorical values.
- Reviewed numerical and date columns.

### 3. Data Cleaning
- Removed exact duplicate rows.
- Standardized text values and capitalization.
- Converted date columns into datetime format.
- Converted numerical columns into appropriate data types.
- Handled invalid negative quantities.
- Validated discount values.
- Converted percentage-formatted discounts into decimal values.
- Identified invalid shipping dates.

### 4. Data Integration
- Standardized identifier formats.
- Combined historical orders with the new daily batch.
- Validated Customer IDs and Product IDs against their master datasets.
- Merged customer and product information using left joins.

### 5. Final Validation
- Identified missing and unmatched identifiers.
- Preserved the original cleaned datasets.
- Created a separate dataset containing only records with valid customer and product references.
- Checked the final dataset for duplicate rows and invalid identifiers.

### 6. Export
Exported the cleaned datasets, combined dataset, enriched dataset, final valid orders, and final quality report into CSV files.

## Final Results

| Metric | Result |
|---|---:|
| Original Orders | 1,020 |
| Original New Daily Batch | 333 |
| Cleaned Orders | 1,018 |
| Cleaned New Daily Batch | 329 |
| Combined Orders | 1,347 |
| Enriched Orders | 1,347 |
| Final Valid Orders | 1,330 |
| Excluded Records | 17 |
| Duplicate Rows in Combined Orders | 0 |

The 17 excluded records consisted of:
- 7 records with missing Customer IDs.
- 4 records with unmatched Customer IDs.
- 6 records with unmatched Product IDs.

The final valid dataset contains no missing or unmatched Customer IDs or Product IDs.

**Note:** Other missing values, such as missing shipping dates or numerical values, may still exist in the final dataset. The final validation specifically focuses on customer and product references and duplicate rows.

## Project Files

- `Data_Cleaning_Project.ipynb` — Main Jupyter Notebook containing the complete workflow.
- `Data before cleaning.zip` — Original input datasets.
- `cleaned_data_final.zip` — Exported cleaned datasets and final quality report.
- `PROJECT_DOCUMENTATION.md` — Detailed explanation of the project workflow.

## How to Run the Project

1. Open [Google Colab](https://colab.research.google.com/).
2. Upload `Data_Cleaning_Project.ipynb`.
3. Run the notebook cells in order.
4. When prompted, upload `Data before cleaning.zip`.
5. The notebook will process the datasets and generate `cleaned_data_final.zip`.
6. Download the final ZIP file containing the exported CSV datasets.

## Final Output

The most important output is:

`final_valid_orders.csv`

It contains 1,330 records with valid customer and product references and is ready for further analysis.

