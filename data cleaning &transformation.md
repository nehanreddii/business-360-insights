# Data Cleaning Process

## Step 1: Using Power Query

In the initial stage of data cleaning, we employ Power Query to ensure that our datasets are refined and standardized for further analysis. This step involves several key tasks:

- **Removing Null Values**: We meticulously identify and eliminate any null or missing values present in the datasets. This ensures data completeness and accuracy, laying a solid foundation for subsequent analyses.

- **Verifying Data Accuracy**: Thorough data validation checks are conducted to verify the accuracy and integrity of the information. This involves cross-referencing against established benchmarks or business rules to identify and rectify any discrepancies.

- **Eliminating Unnecessary Columns**: We streamline the datasets by removing redundant or extraneous columns that do not contribute to the analysis process. This simplifies the data structure and enhances efficiency during subsequent operations.

- **Assigning Appropriate Data Types**: Each column is carefully assessed, and appropriate data types are assigned based on the nature of the information it contains. This ensures consistency and facilitates seamless data manipulation across different tools and platforms.

- **Formatting Date and Time**: Date and time information is standardized and formatted uniformly across the dataset. This not only improves readability but also facilitates chronological analysis and time-series forecasting.

By diligently performing these tasks using Power Query, we ensure that our datasets are clean, standardized.
  
## data transformation | creating tables| creating measured columns -|power query|

- **Creating New Table**: dim_date
  - Columns: date, start of the month, fiscal year
  - Syntax: 
    ```
    = Table.AddColumn(#"start of the month", "fiscal_year", each Date.Year(Date.AddMonths([month],4)))
    ```

**Creating New Table**: Fact_Actuals_Estimates

Columns:
- Date
- Product_Code
- Customer_Code
- Quantity
- Gross_Sales_Amount
- Net_Sales_Amount

## Tables Grouped by Categories

### Dimension Tables
- dim_tables
- dim_products
- dim_market
- dim_date
- dim_customers

### Fact Tables
- fact_forecast_monthly
- fact_sales_monthly
- fact_actuals_and_estimate's

### Supporting Tables
- freight_cost
- manufacturing_cost
- post_invoice_deductions
- pre_invoice_deductions
  
*Note: Data sourced from CodeBasics Data Analytics Bootcamp.*
