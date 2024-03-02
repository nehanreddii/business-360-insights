# data modeling 
  - connecting dimention tables to fact tables |star schema & snow flake|.


## dax formulas 
### key measures :
 net sales - $ns= NS $ = SUM(fact_actuals_estimates[net_sales_amount])
------------------------------------------------------------------------
 Sales Qty = CALCULATE(
       SUM(fact_actuals_estimates[Qty]),
        fact_actuals_estimates[date]<=MAX(LastSalesMonth[LastSalesMonth]))
---------------------------------------------------------------------------------

 gross sales- GS $ = SUM(fact_actuals_estimates[gross_sales_amount])
------------------------------------------------------------------------------------- 
 net sales last year - NS $ LY = CALCULATE([NS $], 
                                   SAMEPERIODLASTYEAR(dim_date[date]))
                                   
-------------------------------------------------------------------------------------------------                                   
post invoice deductions- Post Invoice Deduction $ = SUM(fact_actuals_estimates[post_invoice_deductions_amount])
-----------------------------------------------------------------------------------------------------------------------

post invoice  other deductions - Post Invoice other Deduction $ = SUM(fact_actuals_estimates[post_invoice_other_deductions_amount])
---------------------------------------------------------------------------------------------------------------------------------------

total post invoice deductions - Total Post Invoice Deduction $ = 'Key Measures'[Post Invoice Deduction $] + 'Key Measures'[Post Invoice other Deduction $]
--------------------------------------------------------------------------------------------------------------------------------------------------------------
Manufacturing cost- Manufacturing Cost $ = SUM(fact_actuals_estimates[manufacturing_cost])
------------------------------------------------------------------------------------------------------------------------------------------------------------
frieght cost - Freight Cost $ = SUM(fact_actuals_estimates[freight_cost])


other cost - Other Cost $ = SUM(fact_actuals_estimates[other_cost])


total cogs - Total COGS $ = 'Key Measures'[Manufacturing Cost $] + [Freight Cost $] + 'Key Measures'[Other Cost $]

Other Operational Expense $ = SUM(fact_actuals_estimates[other_operational_expense])



gross margin - GM $ = [NS $]-'Key Measures'[Total COGS $]

 GM % = DIVIDE([GM $],[NS $],0)

GM % LY = CALCULATE([GM %],SAMEPERIODLASTYEAR(dim_date[date]))

Net Profit $ = [GM $]-[Operational Expense $]

Net Profit % = DIVIDE([Net Profit $], [NS $], 0)

Net Profit % LY = CALCULATE([Net Profit %],SAMEPERIODLASTYEAR(dim_date[date]))

NP Target $ = SUM(NsGmTarget[np_target])

NP % Target = DIVIDE([NP Target $], SUM(NsGmTarget[ns_target]),0)

-----------------------------------------------------------------------------------------------------------------------
P & L values = 

var res = SWITCH(
TRUE(),
MAX('P & L Rows'[Order]) =1, [GS $]/1000000,
MAX('P & L Rows'[Order])  =2, [Pre Invoice Deduction $]/1000000,
MAX('P & L Rows'[Order])  =3, [NIS $]/1000000,
MAX('P & L Rows'[Order])  =4, [Post Invoice Deduction $]/1000000,
MAX('P & L Rows'[Order]) =5, [Post Invoice other Deduction $]/1000000,
MAX('P & L Rows'[Order])=6,[Post Invoice Deduction $]/1000000+[Post Invoice other Deduction $]/1000000,
MAX('P & L Rows'[Order])  =7, [NS $]/1000000,
MAX('P & L Rows'[Order]) =8, [Manufacturing Cost $]/1000000,
MAX('P & L Rows'[Order])  =9, [Freight Cost $]/1000000,
MAX('P & L Rows'[Order])  =10, [Other Cost $]/1000000,
MAX('P & L Rows'[Order])  =11,[Total COGS $]/1000000,
MAX('P & L Rows'[Order]) =12, [GM $]/1000000,
MAX('P & L Rows'[Order]) =13, [GM %]*100,
MAX('P & L Rows'[Order]) =14, [GM / Unit],
MAX('P & L Rows'[Order]) =15, [Operational Expense $]/1000000,
MAX('P & L Rows'[Order]) =16, [Net Profit $]/1000000,
MAX('P & L Rows'[Order]) =17, [Net Profit %]*100)

return 
IF(HASONEVALUE('P & L Rows'[Description]), res, [NS $]/1000000)

--------------------------------------------------------------------

P & L vales  LY = CALCULATE([P & L values], SAMEPERIODLASTYEAR(dim_date[date]))

P & L Target = 
var res = SWITCH(
TRUE(),
MAX('P & L Rows'[Order])  =7, [NS Target $]/1000000,
MAX('P & L Rows'[Order]) =12, [GM Target $]/1000000,
MAX('P & L Rows'[Order]) =13, [GM % Target]*100,
MAX('P & L Rows'[Order]) =17, [NP % Target]*100)

return 
IF(HASONEVALUE('P & L Rows'[Description]), res, [NS Target $]/1000000)

--------------------------------------------------------------------------------


key performance indicators for this project

Net Error = [Forecast Qty]- 'Key Measures'[Sales Qty]

Net Error % = DIVIDE([Net Error], [Forecast Qty],0)

Net Error LY = CALCULATE([Net Error],
                SAMEPERIODLASTYEAR(dim_date[date]))
                
------------------------------------------------------------------------------
 ABS Error = 

SUMX(DISTINCT(dim_date[month]),

SUMX(DISTINCT(dim_product[product_code]),  

ABS([Net Error])
)
)   
---------------------------------------------------------------------------------

ABS Error % = DIVIDE('Key Measures'[ABS Error],[Forecast Qty],0)
------------------------------------------------------------------------------

ABS Error LY = CALCULATE('Key Measures'[ABS Error],
SAMEPERIODLASTYEAR(dim_date[date]))


---------------------------------------------------------------------------------












                                   



 
