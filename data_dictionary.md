# Maven Toys Data Dictionary

This document defines the core attributes and metrics used in the Sales & Inventory Analysis dashboard to ensure clarity for all stakeholders.

| Table | Column | Description |
| :--- | :--- | :--- |
| **Sales** | `Order_ID` | Unique transaction identifier for every customer purchase. |
| **Sales** | `Quantity` | The number of units sold in a single transaction. |
| **Products** | `Product_Cost` | The raw manufacturing/acquisition cost of the item. |
| **Products** | `Product_Price` | The retail price (MSRP) the customer pays. |
| **Stores** | `Store_Location` | The geographic setting of the store (Airport, Downtown, etc.). |
| **Calendar** | `Date` | The primary date field used for all time-intelligence calculations. |
| **Measures** | `Total Profit` | Calculated as: Revenue minus (Quantity * Product Cost). |
