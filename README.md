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

## **7 💡 Key Business Findings & Actionable Insights**
Through data visualization and trend analysis, I identified three primary areas for operational optimization:
### **📈 Peak Profitability Window**
* **Finding:** Revenue and Profit reached an annual peak in **March**, driven primarily by the **Art & Crafts** category.
* **Actionable Insight:** Increase marketing spend and safety stock levels for this category 30 days prior to the March peak to maximize capture and prevent stockouts.
### **🏢 Location-Specific Demand**
* **Finding:** **Downtown** locations significantly outperform **Airport** stores in total order volume; however, Airport stores maintain a higher **Profit Margin** per transaction.
* **Regional & Store-Level Analysis:** By selecting a location type from the slicer, users can identify every specific store location and city operating within that category
     * Airport: Filter to see performance across all airport-based stores to track travel-hub sales.
     * Commercial: Identify specific stores located in business districts and industrial zones.
     * Downtown: Drill down into high-density urban centers to see city-specific results.
     * Residential: View all neighborhood-based stores to understand local community shopping habits.
* **Actionable Insight:** Prioritize high-margin, "grab-and-go" inventory for Airport locations, while utilizing volume-based bundle promotions for Downtown stores to capitalize on high foot traffic.
### **📉 The Q3 Performance Slump**
* **Finding:** There is a significant performance dip across all store types during **August and September**.
* **Actionable Insight:** This period identifies a strategic window for "Back-to-School" or "End of Summer" clearance events to liquidate aging inventory and maintain healthy cash flow.

## **8 Technical Challenges & Solutions**
Building a professional-grade dashboard presented several technical hurdles that required advanced DAX and Data Modeling logic:
### **1. Engineering a Custom Time-Intelligence Schema**
* **Challenge:** The raw sales data lacked comprehensive date attributes (such as "Start of Month" or "Day Name"), preventing accurate Year-over-Year (YoY) or Month-over-Month (MoM) trend analysis.
* **Solution:** I engineered a dedicated **Calendar Dimension Table** using Power Query. By establishing a **1:Many relationship** with the Sales fact table, I enabled the use of Time-Intelligence DAX functions like `TOTALYTD` and `DATEADD`.
### **2. Implementing Context-Aware Dynamic Headers**
* **Challenge:** When users filtered by city or store, the dashboard title remained static, leading to potential confusion regarding the active data segment.
* **Solution:** I developed a nested DAX measure using `SELECTEDVALUE`. This measure "listens" to the slicer selection and updates the title string dynamically. If no specific filter is applied, it defaults to **"All Locations"** to ensure constant clarity for the stakeholder.
### **3. Optimizing UX with Reset Logic**
* **Challenge:** Slicers in Power BI can be "sticky," and manually clearing multiple filter hierarchies (Location > City > Store) created a friction-heavy user experience.
* **Solution:** I utilized **Bookmarks and Action Buttons**. By capturing the "Default State" of the data model, I mapped a **Reset Button** to that specific bookmark, allowing users to revert to the high-level view in a single click.

## **9. 🛠️ Tools Used
* **Power BI Desktop:** (Data Modeling, DAX, Visualization)
* **Power Query:** ETL processes (Data Cleaning, Merging, and Transformation).
* **GitHub:** Version control and project documentation.
