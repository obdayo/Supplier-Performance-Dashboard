Supplier Performance Dashboard
---

<img width="660" alt="image" src="https://github.com/user-attachments/assets/97ba2b6d-8b7f-4a9d-a14e-c6b04a2173c7">

**Table of Content:**
---
* [Problem of Statement](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#problem-of-statement)
* [Data Source](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#data-source)
* [Data Tranformation](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#data-transformation)
* [Data Modelling](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#data-modelling)
* [DAX](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#dax)
* [Data Visualization Dashboard](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#data-visualization-dashboard)
* [Insights](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#insights)

**Problem of Statement:**
---
Enterprise Manufacturers Ltd is facing significant challenges in managing and assessing the quality of raw materials provided by various suppliers. The absence of a centralized procurement system has led to inconsistencies across multiple plants in terms of supplier performance and material quality, resulting in inefficiencies and operational downtime. Currently, there is no standardized method to validate the quality of goods supplied by different vendors, nor a consistent approach across plants for evaluating supplier performance.

The company's programme management team has identified a need to centralize and evaluate supplier quality, and recent efforts have been made to consolidate data on materials, defects, and vendors. This includes data on defective materials and the corresponding downtime caused by these defects. However, the lack of proper data visualization and analysis tools has hindered the company’s ability to make informed decisions about supplier performance.

The primary issues include:

* Inconsistent vendor performance evaluation across different plants.
* Unidentified patterns in defective materials and their impact on production downtime.
* Difficulty in identifying the combination of materials and vendors that consistently underperform.
* Enterprise Manufacturers Ltd is in the process of adopting Power BI for data analysis and visualization, and management has enlisted experts to help model the consolidated data and address key questions around vendor and plant performance. The business aims to use this data to identify problem areas, streamline procurement processes, and enhance supplier relationships.

The key business questions that need to be addressed include:

* Which vendors or plants are contributing to the highest defect quantities?
* Which vendors or plants are responsible for the most downtime?
* Are there specific material and vendor combinations that consistently perform poorly?
* Are there specific vendor and plant combinations that consistently perform poorly?
* How do the same vendor and material combinations perform across different plants?
  
This analysis will provide critical insights to help management improve procurement strategies, reduce defects, minimize downtime, and optimize overall operational performance.

This dashboard equips decision-makers with actionable insights to drive process improvements and enhance overall operational efficiency.

**Data Source**
---
The Dataset used for this task was presented by [Tina Okonkwo](https://www.linkedin.com/in/augustina-okonkwo/)

Dataset: [Supplier Quality Dataset](https://github.com/obdayo/Supplier-Performance-Dashboard/blob/main/supplier%20data.xlsx)

**Data Transformation**
---

The **Supplier Quality dataset**, comprising **9 columns** and **5,227 rows**, was successfully transformed and loaded into Microsoft Power BI Desktop making use of an excel document for advanced modeling and analysis.

Key steps taken during the data transformation process in Power Query Editor included:

* **Supplier Quality Fact Table Creation:** Developed a central fact table along with six dimension tables to optimize the data model for efficient analysis.

* **Data Cleaning and Validation:** Removed irrelevant columns and rows, ensuring the dataset was lean and focused. Each column was validated for accuracy, ensuring appropriate data types were applied across the tables.

* **Duplicate Management:** Identified and removed duplicate entries within the dimension tables to maintain data integrity and accuracy.

* **Indexing for Table Linking:** Introduced an index column in each dimension table to serve as unique IDs, ensuring precise table relationships.

* **Data Integration with Merge Queries:** Leveraged Power Query’s Merge functionality to establish relationships between the dimension tables and the fact table via inner joins, using the newly created ID fields for optimal data linkage.

This structured approach ensured a clean, efficient, and well-modeled dataset, ready for insightful reporting and analysis within Power BI.

**Data Modelling**
---

<img width="382" alt="image" src="https://github.com/user-attachments/assets/e56d9b37-1d7e-4999-b75b-1e556cdd54ab">

The image represents a star schema data model within Microsoft Power BI. In this model, there is a central fact table—titled "Supplier Quality Fact Table" that is surrounded by dimension tables. This design is optimized for reporting and querying. 

Here’s a breakdown of the elements:

**Fact Table (Central Table):**

**Supplier Quality Fact Table:** This is the central table that contains key performance metrics related to supplier quality, and it includes foreign keys that link to various dimension tables. 

Columns include:
* Category
* Category_ID
* Date
* Defect
* Defect Type
* Material Type
* Material_ID
  
**Dimension Tables (Surrounding Tables):**

* **Defect Type:** This table contains defect type information with the column "Defect_Type_ID" to uniquely identify each type.
  
* **Defect:** Contains defect data with "Defect_ID" for unique identification.
  
* **Category:** Holds data related to different categories with "Category_ID" for reference.
  
* **Date Table:** A date dimension table, often used for time-based analysis.

This table includes:
* Date
* Month
* Month Name
* Quarter

* **Plant:** Contains plant location information, with "Plant_ID" used for identification.

* **Material Type:** This dimension contains information about material types with "Material_Type_ID" for unique identification.

* **Vendor:** The vendor information is stored here with "Vendor_ID" as the unique identifier.

**Relationships:**

The lines connecting the fact table to the dimension tables represent relationships. These relationships are typically one-to-many, meaning that a single value in the dimension table (e.g., one vendor) can relate to multiple rows in the fact table (e.g., multiple instances of that vendor's products).
The key fields (e.g., "Vendor_ID," "Defect_ID," etc.) in the dimension tables are linked to corresponding fields in the fact table to allow for proper data joins.

**Purpose:**

This star schema is designed for efficient querying and analysis in Power BI. It allows users to easily slice and dice supplier quality data by various dimensions like defect type, vendor, material type, and time periods (using the Date Table).

**DAX**
---
Measures used in all visualization are:

**Defect Quantity:**

* Total Defect Quantity = SUM('Supplier Quality Fact Table'[Total Defect Qty])
  * Purpose: Calculates the total quantity of defects across all records.

* Defect Quantity SPLY = CALCULATE([Total Defect Quantity],SAMEPERIODLASTYEAR('Date Table'[Date]))
  * Purpose: Calculates the total defect quantity for the same period in the previous year to allow for YoY comparisons.

* Defect quantity PM = CALCULATE([Total Defect Quantity],PARALLELPERIOD('Date Table'[Date],-1,MONTH))
  * Purpose: Calculates the total defect quantity for the previous month, allowing for MoM analysis.

* Month on Month DQ = DIVIDE([Total Defect Quantity] - [Total Defect quantity PM],[Total Defect quantity PM])
  * Purpose: Calculates the percentage change in defect quantity compared to the previous month.

* Year on Year DQ = DIVIDE([Total Defect Quantity] - [Defect Quantity SPLY],[Defect Quantity SPLY])
  * Purpose: Calculates the percentage change in defect quantity compared to the same period last year.

* Rank Vendor by Defect Quantity = 
         VAR _tvdq = 
             IF(ISINSCOPE(Vendor[Vendor]),(RANKX(ALL(Vendor[Vendor]),[Total Defect Quantity], ,DESC)))
         VAR _bvdq = 
             IF(ISINSCOPE(Vendor[Vendor]),(RANKX(ALL(Vendor[Vendor]),[Total Defect Quantity], ,ASC)))
         VAR _rankig = 
             IF(SELECTEDVALUE(TopBottom[Value]) = "Top",_tvdq,_bvdq)
         RETURN
            IF(_rankig <= 'Top N Parameter'[Top N Parameter Value],[Total Defect Quantity])
  * Purpose: Ranks vendors based on their total defect quantity. The ranking can show either "Top N" or "Bottom N" vendors depending on the selection in the TopBottom slicer and compares vendors' performance dynamically.


**Downtime Hours:**

* Total Downtime Hours = DIVIDE([Total Downtime Minutes],60)
  * Purpose: Converts total downtime minutes into hours for easier interpretation.

* Downtime Hours SPLY = CALCULATE([Total Downtime Hours],SAMEPERIODLASTYEAR('Date Table'[Date]))
  * Purpose: Calculates the total downtime hours for the same period last year, enabling YoY comparisons.

* Downtime Hours PM = CALCULATE([Total Downtime Hours],PARALLELPERIOD('Date Table'[Date],-1,MONTH))
  * Purpose: Calculates the total downtime hours for the previous month, allowing for MoM analysis.

* Month on Month DH = DIVIDE([Total Downtime Hours] - [Downtime Hours PM],[Downtime Hours PM])
  * Purpose: Calculates the percentage change in downtime hours compared to the previous month.

* Year on Year DH = DIVIDE([Total Downtime Hours] - [Downtime Hours SPLY],[Downtime Hours SPLY])
  * Purpose: Calculates the percentage change in downtime hours compared to the same period last year.

* Rank Vendor by Downtime Hours = 
         VAR _tvdth = 
             IF(ISINSCOPE(Vendor[Vendor]),(RANKX(ALL(Vendor[Vendor]),[Total Downtime Hours], ,DESC)))
         VAR _bvdth = 
             IF(ISINSCOPE(Vendor[Vendor]),(RANKX(ALL(Vendor[Vendor]),[Total Downtime Hours], ,ASC)))
         VAR _rankig = 
             IF(SELECTEDVALUE(TopBottom[Value]) = "Top",_tvdth,_bvdth)
         RETURN
            IF(_rankig <= 'Top N Parameter'[Top N Parameter Value],[Total Downtime Hours])
  * Purpose: Ranks vendors based on their total downtime hours, either showing "Top N" or "Bottom N" vendors as per user selection.

This approach supports detailed, dynamic reporting and allows stakeholders to monitor performance trends and identify top and bottom performers effectively.

**Data Visualization Dashboard**
---

Data visualization for the data analysis (DAX) was done in Microsoft Power BI Desktop:

Dashboard: [View Dashboard](https://app.powerbi.com/view?r=eyJrIjoiNTZkMmQ3NzUtYjY3ZC00YTBhLTgwMGMtMWFlNWU0MTcwNTJjIiwidCI6IjZiMTZjODcyLWY0MTQtNDZjZS1hNTNkLWJlNjRiYjA3NTk0NSJ9&pageName=ReportSectiona63f8e1bcaff3261d734)

Shows visualizations from Supplier Quality & Performance Dashboard:

<p>
    <h3 align ="center">Overview</h3>
</p>


<img width="750" alt="image" src="https://github.com/user-attachments/assets/96816e95-722b-4505-a8e5-dd1b82f7f5b0">

<p>
    <h3 align ="center">Vendor Performance</h3>
</p>

<img width="750" alt="image" src="https://github.com/user-attachments/assets/c8524408-c217-469a-8c31-ee89e4755863">

<p>
    <h3 align ="center">Material Performance</h3>
</p>

<img width="750" alt="image" src="https://github.com/user-attachments/assets/440e1b5d-27af-423a-a8ce-cd9b6abe0353">

**Insights:**
---

The Power BI dashboard provides a comprehensive view of supplier performance, focusing on defect quantity, downtime hours, and cost impacts.

Key highlights include:

* **Defect Quantity:** 2.60 billion defects, with a 4.25% increase from last month and a 123.32% spike compared to last year.

* **Downtime Hours:** 215.81K hours, translating to a downtime cost of $2.16M, with similar monthly and yearly increases.
* 
* **Worst Performers:** Vendors like Avamm and plants like Charles City are key underperformers, with high defect rates.
* 
* **Trends:** Monthly defect trends show a peak in October, with significant impact (31.71%) and high rejection rates (28.43%).

These insights highlight critical areas of inefficiency in manufacturing processes.








