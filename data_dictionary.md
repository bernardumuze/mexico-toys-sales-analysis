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

* **Total Revenue:** `SUMX(Sales, Sales[Units] * RELATED(Products[Product_Price]))`
* **Total Profit:** `[Total Revenue] - SUMX(Sales, Sales[Units] * RELATED(Products[Product_Cost]))`
* **Selected Location Title:** A dynamic measure that updates the report header based on user slicer selections.

---
