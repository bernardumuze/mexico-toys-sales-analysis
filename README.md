# Maven Toys: Strategic Sales & Inventory Analysis

## Maven Toys: Strategic Sales & Inventory Analysis.
This project analyzes Maven Mexico Toys retail data and transforms it into an intuitive business intelligence tool. The aim is to provide stakeholders with clear insights to help them understand revenue trends, product performance, and how to improve inventory at different store locations.

### **Key Business Metrics (YTD)**
* **Total Revenue:** $6.96M
* **Total Profit:** $1.82M
* **Total Orders:** 408,417

---

## 🖼️ Dashboard Preview
<img width="1367" height="772" alt="Mexico Toys-Strategic Sales   Inventory Analysis" src="https://github.com/user-attachments/assets/548bffba-edec-4d56-a80f-78c4d0f2ded7" />
---

## **1. Data Connection & Profiling**
* Connected to **sales, products, stores, and calendar** CSV files.
* Reviewed table columns for blank or null values and confirmed accurate data types.
* Identified primary and foreign keys and profiled the data to understand transaction volume and price ranges.
* Added calculated columns in the calendar table for **'start of month'** and **'start of week'**.
  
## **2. Relational Data Modeling**
Implemented a Star Schema to optimize performance and usability.

<img width="1086" height="631" alt="Star Schema Data Model for Maven Toys Sales Analysis" src="https://github.com/user-attachments/assets/e5962b8a-bd22-4a3a-9d9f-d369381a9973" />

* Loaded tables into the data model and established relationships from the sales table to the products, stores, and calendar tables.
* Developed a **Star Schema** following data modeling best practices with 1:many relationships.
* Created a date hierarchy and hid foreign keys in the sales table to streamline the report view.

## **3. DAX Calculations & Advanced Fields**
To drive the KPI cards and trend lines, I developed the following measures:

**1. Total Revenue**
Used to calculate the gross sales across all transactions.
`Total Revenue = SUMX(Sales, Sales[Quantity] * RELATED(Products[Product_Price]))`

**2. Total Profit**
Calculated by subtracting the product cost from the sales price for every unit sold.
`Total Profit = [Total Revenue] - SUMX(Sales, Sales[Quantity] * RELATED(Products[Product_Cost]))`

**3. Total Orders**
A simple count of unique transactions to track volume.
`Total Orders = COUNT(Sales[Order_ID])`

**4. Revenue YTD (Year-to-Date)**
This time-intelligence measure allows stakeholders to track cumulative sales performance from the start of the fiscal year to the current date.
`Revenue YTD = TOTALYTD(SUM(Sales[Revenue]), 'calendar'[Date])`

**5. Total Orders YTD (Year-to-Date)**
Tracks the total volume of transactions handled by the retail chain from the start of the year.
`Total Orders YTD = TOTALYTD(COUNT(sales[Store_ID]), 'calendar'[Date])`

**6. Total Profit YTD**
Calculates the cumulative profit from the start of the year, providing a clear view of bottom-line growth.
`Total Profit YTD = TOTALYTD(SUM(Sales[Profit]), 'calendar'[Date])`

**7. Last Month Revenue**
This comparative measure uses `CALCULATE` and `DATEADD` to retrieve sales data from the previous month, enabling the calculation of growth percentages.
`Last Month Revenue = CALCULATE([Total Revenue], DATEADD('calendar'[Date], -1, MONTH))`

**8. Total OrdersTarget**
Used as a benchmark to compare current month order volume against the previous month's performance.
`Total OrdersTarget = CALCULATE([Total Orders], DATEADD('calendar'[Date], -1, MONTH))`

**9. Total Profit Target**
Measures profitability against the previous 30-day period to identify margin fluctuations.
 `Total Profit Target = CALCULATE([Total Profit], DATEADD('calendar'[Date], -1, MONTH))

**10. Selected Location Title**
 Enhances report interactivity by dynamically updating the chart header based on user slicer selections (e.g., "Revenue Trends for Mexico City" or "Revenue Trends for All Locations").
 `Selected Location Title = "Revenue Trends for " & SELECTEDVALUE(stores[Store_City], SELECTEDVALUE(stores[Store_Location], "All Locations"))

## **4. Interactive Reporting**
* Built a multi-visual dashboard featuring:
    * **KPI Cards:** High-level summary of YTD performance and monthly trends.
    * **Bar Chart:** Total orders broken down by product category (Art & Crafts, Toys, etc.).
    * **Line Chart:** Monthly revenue trends showing performance peaks and valleys.
    * **Interactive Slicer:** Allowing users to filter the entire report by store location.
 
## **5. Interactive Button Navigation**
Instead of complicating the report with multiple pages, I utilized **Power BI Buttons and Bookmarks** to create a high-performance, single-page application experience:
* **🔄 Reset Filters Button:** Allows users to instantly clear all slicer selections (Location, City, and Store) and return to the high-level "All Locations" view with a single click.
* **🔘 Interactive Slicer Panel:** I implemented a toggle button that expands and collapses the filter menu. This maximizes the "real estate" for data visualizations while keeping powerful filtering tools accessible.

## **6. Dynamic "Drill-Through" Filtering**
The dashboard features a sophisticated hierarchical filtering system, allowing users to move from broad categories down to specific operational units:
* **Step 1 (Location Type):** Users select a broad category (e.g., *Downtown*).
* **Step 2 (Cascading Slicer):** The "Store Name" slicer automatically updates via the relational model to show **only** the stores belonging to that category (e.g., *Guadalajara 1, Monterrey 2*).
* **Step 3 (Dynamic Context):** The report header instantly updates to reflect the selection via the `Selected Location Title` measure, providing immediate confirmation of the data context.
      
## **7.💡 Key Business Insights & Recommendations**
* **Top Category:** Art & Crafts drives the highest order volume across all store types.
* **Seasonal Trends:** Revenue reached its peak in **March**, followed by a steady plateau and a sharp decline in **August/September**.
* **Operational Opportunity:** By filtering for specific locations like "Airport" or "Downtown," managers can identify location-specific inventory needs.
* **Regional & Store-Level Analysis:** By selecting a location type from the slicer, users can identify every specific store location and city operating within that category
     * Airport: Filter to see performance across all airport-based stores to track travel-hub sales.
     * Commercial: Identify specific stores located in business districts and industrial zones.
     * Downtown: Drill down into high-density urban centers to see city-specific results.
     * Residential: View all neighborhood-based stores to understand local community shopping habits.

## **8. 🛠️ Tools Used
* **Power BI Desktop:** (Data Modeling, DAX, Visualization)
* **Power Query:** ETL processes (Data Cleaning, Merging, and Transformation).
* **GitHub:** Version control and project documentation.
