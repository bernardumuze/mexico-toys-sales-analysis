---

## 📖 Data Dictionary
To ensure data transparency and facilitate a smooth onboarding experience for stakeholders, I have defined the core attributes used in this analysis.

### **1. Sales Table (Fact Table)**
*The core transactional table containing every sale made by Maven Toys.*

| Column | Description |
| :--- | :--- |
| `Sale_ID` | A unique identifier for each individual transaction. |
| `Date` | The transaction date; linked to the **Calendar** table for time-intelligence. |
| `Store_ID` | Foreign key linking to the **Stores** table to identify the sale location. |
| `Product_ID` | Foreign key linking to the **Products** table to identify the item sold. |
| `Units` | The total quantity of items sold in a single order row. |

### **2. Products Table (Dimension Table)**
*Details about the toy inventory, used for categorization and financial calculations.*

| Column | Description |
| :--- | :--- |
| `Product_Name` | The specific name of the toy (e.g., Colorbuds, Lego Terriers). |
| `Product_Category` | High-level grouping (e.g., Art & Crafts, Toys, Games). |
| `Product_Cost` | The price Maven Toys pays to acquire/manufacture one unit. |
| `Product_Price` | The retail price paid by the customer (MSRP). |

### **3. Stores Table (Dimension Table)**
*Information regarding the physical retail locations across Mexico.*

| Column | Description |
| :--- | :--- |
| `Store_Name` | The name of the specific branch (e.g., Mexico City 1). |
| `Store_City` | The city where the store is located. |
| `Store_Location` | Categorization of the store setting (Airport, Downtown, Commercial, Residential). |
| `Store_Open_Date` | The date the store was officially opened for business. |

### **4. Calendar Table (Dimension Table)**
*A custom-engineered table used to enable Year-over-Year (YoY) and Month-over-Month (MoM) reporting.*

| Column | Description |
| :--- | :--- |
| `Date` | The unique date key for the entire model. |
| `Start of Month` | Normalizes all dates to the first of the month for trend visualization. |
| `Day Name` | Identifies the day (Monday, Tuesday, etc.) to analyze shopping patterns. |

### **5. Key Calculated Measures (DAX)**
*The business logic layer used to drive dashboard visuals.*

## 🔢 Key Calculated Measures (DAX)
I developed a comprehensive DAX library to drive KPI performance tracking, time-intelligence analysis, and dynamic report headers.

| Measure Name | DAX Formula | Business Purpose |
| :--- | :--- | :--- |
| **Total Revenue** | `$Total Revenue = SUMX(Sales, Sales[Quantity] * RELATED(Products[Product_Price]))$` | Calculates gross sales across all transactions. |
| **Total Profit** | `$Total Profit = [Total Revenue] - SUMX(Sales, Sales[Quantity] * RELATED(Products[Product_Cost]))$` | Calculates net profit by subtracting unit costs from sales. |
| **Total Orders** | `$Total Orders = COUNT(Sales[Order_ID])$` | Tracks the total volume of unique customer transactions. |
| **Revenue YTD** | `$Revenue YTD = TOTALYTD([Total Revenue], 'calendar'[Date])$` | Tracks cumulative revenue from the start of the year. |
| **Total Orders YTD** | `$Total Orders YTD = TOTALYTD([Total Orders], 'calendar'[Date])$` | Tracks cumulative transaction volume for the year. |
| **Total Profit YTD** | `$Total Profit YTD = TOTALYTD([Total Profit], 'calendar'[Date])$` | Tracks year-to-date bottom-line growth. |
| **Last Month Revenue** | `$Last Month Revenue = CALCULATE([Total Revenue], DATEADD('calendar'[Date], -1, MONTH))$` | Enables Month-over-Month (MoM) growth comparisons. |
| **Total Orders Target** | `$Total Orders Target = CALCULATE([Total Orders], DATEADD('calendar'[Date], -1, MONTH))$` | Sets a benchmark based on the previous month's performance. |
| **Total Profit Target** | `$Total Profit Target = CALCULATE([Total Profit], DATEADD('calendar'[Date], -1, MONTH))$` | Measures current profitability against the previous 30 days. |
| **Selected Location Title** | `Selected Location Title = "Revenue Trends for " & SELECTEDVALUE(stores[Store_City], "All Locations")` | Dynamically updates the report title based on user filters. |

---
