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
* Data Vizualization Dashboard
* Insights

**Problem of Statement:**
---
This project contains the Supplier Quality & Performance Dashboard created using Power BI for Enterprise Manufacturers that reflects all relevant Key Performance Indicators (KPIs) and Business questions that were answered.

Some key questions the business want answering are;

* Which vendors/plants are causing the greatest defect quantity?
* Which vendors/plants are causing the greatest downtime?
* Is there a particular combination of material and vendor that perform poorly?
* Is there a particular combination of Vendor and plant that performs poorly?
* How does the same vendor and material perform across different plants?

**Data Source**
---
The Dataset used for this task was presented by [Tina Okonkwo](https://www.linkedin.com/in/augustina-okonkwo/)

Dataset: [Supplier Quality Dataset](https://github.com/obdayo/Supplier-Performance-Dashboard/blob/main/supplier%20data.xlsx)

**Data Transformation**
---

The **Supplier Quality dataset**, comprising **9 columns** and **5,227 rows**, was successfully transformed and loaded into Microsoft Power BI Desktop for advanced modeling and analysis.

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








