# Data Modeling

## Connecting Dimension Tables to Fact Tables (Star Schema & Snowflake).

## DAX Formulas

### Key Measures:

- **Net Sales ($NS)**:
  - Calculation: `NS $ = SUM(fact_actuals_estimates[net_sales_amount])`
  
- **Sales Qty**:
  - Calculation: `Sales Qty = CALCULATE(SUM(fact_actuals_estimates[Qty]), fact_actuals_estimates[date] <= MAX(LastSalesMonth[LastSalesMonth])`
  
- **Gross Sales ($GS)**:
  - Calculation: `GS $ = SUM(fact_actuals_estimates[gross_sales_amount])`
  
- **Net Sales Last Year ($NS LY)**:
  - Calculation: `NS $ LY = CALCULATE([NS $], SAMEPERIODLASTYEAR(dim_date[date]))`
  
- **Post Invoice Deductions ($Post Invoice Deduction)**:
  - Calculation: `Post Invoice Deduction $ = SUM(fact_actuals_estimates[post_invoice_deductions_amount])`
  
- **Post Invoice Other Deductions ($Post Invoice Other Deduction)**:
  - Calculation: `Post Invoice Other Deduction $ = SUM(fact_actuals_estimates[post_invoice_other_deductions_amount])`
  
- **Total Post Invoice Deductions ($Total Post Invoice Deduction)**:
  - Calculation: `Total Post Invoice Deduction $ = 'Key Measures'[Post Invoice Deduction $] + 'Key Measures'[Post Invoice Other Deduction $]`
  
- **Manufacturing Cost ($Manufacturing Cost)**:
  - Calculation: `Manufacturing Cost $ = SUM(fact_actuals_estimates[manufacturing_cost])`
  
- **Freight Cost ($Freight Cost)**:
  - Calculation: `Freight Cost $ = SUM(fact_actuals_estimates[freight_cost])`
  
- **Other Cost ($Other Cost)**:
  - Calculation: `Other Cost $ = SUM(fact_actuals_estimates[other_cost])`
  
- **Total COGS ($Total COGS)**:
  - Calculation: `Total COGS $ = 'Key Measures'[Manufacturing Cost $] + [Freight Cost $] + 'Key Measures'[Other Cost $]`
  
- **Other Operational Expense ($Other Operational Expense)**:
  - Calculation: `Other Operational Expense $ = SUM(fact_actuals_estimates[other_operational_expense])`
  
- **Gross Margin ($GM)**:
  - Calculation: `GM $ = [NS $] - 'Key Measures'[Total COGS $]`
  
- **Gross Margin Percentage ($GM %)**:
  - Calculation: `GM % = DIVIDE([GM $], [NS $], 0)`
  
- **Gross Margin Percentage Last Year ($GM % LY)**:
  - Calculation: `GM % LY = CALCULATE([GM %], SAMEPERIODLASTYEAR(dim_date[date]))`
  
- **Net Profit ($Net Profit)**:
  - Calculation: `Net Profit $ = [GM $] - [Operational Expense $]`
  
- **Net Profit Percentage ($Net Profit %)**:
  - Calculation: `Net Profit % = DIVIDE([Net Profit $], [NS $], 0)`
  
- **Net Profit Percentage Last Year ($Net Profit % LY)**:
  - Calculation: `Net Profit % LY = CALCULATE([Net Profit %], SAMEPERIODLASTYEAR(dim_date[date]))`
  
- **NP Target ($NP Target)**:
  - Calculation: `NP Target $ = SUM(NsGmTarget[np_target])`
  
- **NP Percentage Target ($NP % Target)**:
  - Calculation: `NP % Target = DIVIDE([NP Target $], SUM(NsGmTarget[ns_target]), 0)`

### Key Performance Indicators for This Project:

- **Net Error**:
  - Calculation: `Net Error = [Forecast Qty] - 'Key Measures'[Sales Qty]`
  
- **Net Error Percentage**:
  - Calculation: `Net Error % = DIVIDE([Net Error], [Forecast Qty], 0)`
  
- **Net Error Last Year**:
  - Calculation: `Net Error LY = CALCULATE([Net Error], SAMEPERIODLASTYEAR(dim_date[date]))`
  
- **Absolute Error**:
  - Calculation: 
    ```DAX
    ABS Error = SUMX(DISTINCT(dim_date[month]),
                    SUMX(DISTINCT(dim_product[product_code]),  
                    ABS([Net Error])))
    ```
  
- **Absolute Error Percentage**:
  - Calculation: `ABS Error % = DIVIDE('Key Measures'[ABS Error], [Forecast Qty], 0)`
  
- **Absolute Error Last Year**:
  - Calculation: `ABS Error LY = CALCULATE('Key Measures'[ABS Error], SAMEPERIODLASTYEAR(dim_date[date]))`
