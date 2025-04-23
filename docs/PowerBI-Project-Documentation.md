# FarmFresh Market Power BI Project

## Project Overview  
The FarmFresh Market Power BI project aimed to create a comprehensive sales performance dashboard to analyze sales trends, profitability, inventory management, and customer behavior. Stakeholders can use these insights to make informed business decisions.

- **Objective:** Develop an interactive dashboard that visualizes key performance metrics—total sales, profit margin, return rate, and regional sales performance—to optimize business strategies and enhance operational efficiency.  
- **Scope:**  
  - Analyze sales and return data from 1997 to 1998.  
  - Track performance across products, regions, and customer segments.  
  - Provide insights into transaction trends, return rates, and profitability.

---

## Data Sources  
The report utilized the following data files:

- `FarmFresh Market_Customers.csv` – Customer demographics and details.  
- `FarmFresh Market_Products.csv` – Product information including pricing and categories.  
- `FarmFresh Market_Stores.csv` – Store locations and related data.  
- `FarmFresh Market_Regions.csv` – Geographic regions associated with stores.  
- `FarmFresh Market_Calendar.csv` – Calendar data for time intelligence.  
- `FarmFresh Market_Returns_1997-1998.csv` – Data on product returns.  
- `FarmFresh Market_Transactions_1997-1998.csv` – Sales transaction data for 1997 and 1998.

---

## Data Preparation Steps  
1. **Data Cleaning:** Removed null values, enforced correct data types, and formatted columns.  
2. **Data Transformation:** Added calculated columns—`full_name`, `birth_year`, `has_children` in the Customers table and `discount_price` in the Products table.  
3. **Data Merging:** Combined transaction and return data into unified tables (`Transaction_Data`, `Return_Data`).  
4. **Data Modeling:** Established relationships using primary/foreign keys in a star schema.

---

## Data Model & Relationships  
- **Star Schema:** Lookup tables (Customers, Products, Stores, Regions, Calendar) connect to fact tables (Transaction_Data, Return_Data).  
- **One-to-Many Relationships:** Ensured correct cardinality for filter propagation.  
- **Snowflake Schema:** Regions linked through Stores to Transactions.  
- **Inactive Relationships:** Managed the `stock_date` relationship as inactive to avoid ambiguity in date-based measures.

---

## Key Measures & DAX Calculations  
Below are the DAX formulas used in the report:

### Sales Metrics  
| Measure                | Formula                                                    |
|------------------------|------------------------------------------------------------|
| Total Sales            | `SUM(Transaction_Data[SalesAmount])`                       |
| Total Profit           | `SUM(Transaction_Data[Profit])`                            |
| Profit Margin          | `DIVIDE([Total Profit], [Total Sales], 0)`                 |
| Total Transactions     | `COUNT(Transaction_Data[OrderID])`                         |
| Quantity Sold          | `SUM(Transaction_Data[Quantity])`                          |

### Return Analysis  
| Measure                | Formula                                                                                   |
|------------------------|-------------------------------------------------------------------------------------------|
| Quantity Returned      | `SUM(Return_Data[Quantity])`                                                              |
| Total Returns          | `COUNT(Return_Data[ReturnID])`                                                            |
| Return Rate            | `DIVIDE([Quantity Returned], [Quantity Sold], 0)`                                         |
| Weekend Transactions   | `CALCULATE([Total Transactions], Calendar[Weekend] = "Y")`                                |
| % Weekend Transactions | `DIVIDE([Weekend Transactions], [Total Transactions], 0)`                                 |

### Time Intelligence  
| Measure                 | Formula                                                                                 |
|-------------------------|-----------------------------------------------------------------------------------------|
| YTD Revenue             | `TOTALYTD([Total Revenue], Calendar[Date])`                                              |
| 60-Day Revenue          | `CALCULATE([Total Revenue], DATESINPERIOD(Calendar[Date], MAX(Calendar[Date]), -60, DAY))` |
| Last Month Transactions | `CALCULATE([Total Transactions], DATEADD(Calendar[Date], -1, MONTH))`                    |
| Last Month Revenue      | `CALCULATE([Total Revenue], DATEADD(Calendar[Date], -1, MONTH))`                         |
| Last Month Profit       | `CALCULATE([Total Profit], DATEADD(Calendar[Date], -1, MONTH))`                          |
| Last Month Returns      | `CALCULATE([Total Returns], DATEADD(Calendar[Date], -1, MONTH))`                         |

### Revenue & Cost Analysis  
| Measure         | Formula                                                                                     |
|-----------------|---------------------------------------------------------------------------------------------|
| Total Revenue   | `SUMX(Transaction_Data, Transaction_Data[Quantity] * Products[Product_Retail_Price])`       |
| Total Cost      | `SUMX(Transaction_Data, Transaction_Data[Quantity] * Products[Product_Cost])`               |
| Total Profit    | `[Total Revenue] - [Total Cost]`                                                            |
| Profit Margin   | `DIVIDE([Total Profit], [Total Revenue], 0)`                                                |

### Unique Values & Targets  
| Measure           | Formula                                           |
|-------------------|---------------------------------------------------|
| Unique Products   | `DISTINCTCOUNT(Products[ProductName])`            |
| Revenue Target    | `[Last Month Revenue] * 1.05` (5% lift over LM)   |

---

## Visualizations & Report Design  

### Report Pages  
- **Topline Performance:** KPI cards for transactions, profit, and return rate.  
- **Regional Sales:** Map visual showing transactions by store city and country.  
- **Product Performance:** Treemap with drill-through to explore brand/category sales.  
- **Revenue Trends:** Column chart displaying weekly revenue trends for 1998.

### Interactive Elements  
- **Slicers:** Filter by country, region, product category, and date range.  
- **Drill-through:** Enabled on the Treemap to navigate from country → city level.  
- **Bookmarks:** Highlighted key milestones (e.g., “Portland hits 1,000 sales in December”).

---

## Key Insights & Recommendations  
- **Sales Trends:** High-performing product brands and seasonal patterns identified.  
- **Regional Analysis:** Top-performing regions and new market opportunities highlighted.  
- **Return Rate Management:** Strategies recommended to reduce returns and improve margins.  
- **Profitability Enhancement:** Focus on high-margin products and optimize low-performing stores.

---

## Conclusion  
The FarmFresh Market Power BI project demonstrates advanced data modeling, DAX calculations, and visualization skills. The actionable insights empower stakeholders to drive growth, optimize operations, and enhance profitability. :contentReference[oaicite:0]{index=0}
