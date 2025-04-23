# DAX Measures Reference

## Sales Metrics
| Measure               | DAX Formula                                                    |
|-----------------------|----------------------------------------------------------------|
| Total Sales           | `SUM(Transaction_Data[SalesAmount])`                           |
| Total Profit          | `SUM(Transaction_Data[Profit])`                                |
| Profit Margin         | `DIVIDE([Total Profit], [Total Sales], 0)`                     |
| Total Transactions    | `COUNT(Transaction_Data[OrderID])`                             |
| Quantity Sold         | `SUM(Transaction_Data[Quantity])`                              |

## Return Analysis
| Measure                | DAX Formula                                                                      |
|------------------------|----------------------------------------------------------------------------------|
| Quantity Returned      | `SUM(Return_Data[Quantity])`                                                     |
| Total Returns          | `COUNT(Return_Data[ReturnID])`                                                   |
| Return Rate            | `DIVIDE([Quantity Returned], [Quantity Sold], 0)`                                |
| Weekend Transactions   | `CALCULATE([Total Transactions], Calendar[Weekend] = "Y")`                       |
| % Weekend Transactions | `DIVIDE([Weekend Transactions], [Total Transactions], 0)`                        |

## Time Intelligence
| Measure                   | DAX Formula                                                                                          |
|---------------------------|------------------------------------------------------------------------------------------------------|
| YTD Revenue               | `TOTALYTD([Total Revenue], Calendar[Date])`                                                          |
| 60-Day Revenue            | `CALCULATE([Total Revenue], DATESINPERIOD(Calendar[Date], MAX(Calendar[Date]), -60, DAY))`            |
| Last Month Transactions   | `CALCULATE([Total Transactions], DATEADD(Calendar[Date], -1, MONTH))`                                 |
| Last Month Revenue        | `CALCULATE([Total Revenue], DATEADD(Calendar[Date], -1, MONTH))`                                      |
| Last Month Profit         | `CALCULATE([Total Profit], DATEADD(Calendar[Date], -1, MONTH))`                                       |
| Last Month Returns        | `CALCULATE([Total Returns], DATEADD(Calendar[Date], -1, MONTH))`                                      |

## Revenue & Cost Analysis
| Measure         | DAX Formula                                                                                             |
|-----------------|---------------------------------------------------------------------------------------------------------|
| Total Revenue   | `SUMX(Transaction_Data, Transaction_Data[Quantity] * Products[Product_Retail_Price])`                    |
| Total Cost      | `SUMX(Transaction_Data, Transaction_Data[Quantity] * Products[Product_Cost])`                            |
| Total Profit    | `[Total Revenue] - [Total Cost]`                                                                         |
| Profit Margin   | `DIVIDE([Total Profit], [Total Revenue], 0)`                                                            |

## Unique Values & Target Metrics
| Measure          | DAX Formula                                 |
|------------------|---------------------------------------------|
| Unique Products  | `DISTINCTCOUNT(Products[ProductName])`      |
| Revenue Target   | `[Last Month Revenue] * 1.05` (5% lift over previous month) |
