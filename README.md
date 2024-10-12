Supplier Performance Dashboard
---

<img width="660" alt="image" src="https://github.com/user-attachments/assets/97ba2b6d-8b7f-4a9d-a14e-c6b04a2173c7">

**Table of Content:**
---
* [Problem of Statement](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#problem-of-statement)
* [Data Source](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#data-source)
* [Data Tranformation and Modelling](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#data-transformation-and-modelling)
* Data Modelling
* DAX
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

**Data Transformation and Modelling**
---

The **Supplier Quality dataset**, comprising **9 columns** and **5,227 rows**, was successfully transformed and loaded into Microsoft Power BI Desktop for advanced modeling and analysis.

Key steps taken during the data transformation process in Power Query Editor included:

* **Supplier Quality Fact Table Creation:** Developed a central fact table along with six dimension tables to optimize the data model for efficient analysis.

* **Data Cleaning and Validation:** Removed irrelevant columns and rows, ensuring the dataset was lean and focused. Each column was validated for accuracy, ensuring appropriate data types were applied across the tables.

* **Duplicate Management:** Identified and removed duplicate entries within the dimension tables to maintain data integrity and accuracy.

* **Indexing for Table Linking:** Introduced an index column in each dimension table to serve as unique IDs, ensuring precise table relationships.

* **Data Integration with Merge Queries:** Leveraged Power Query’s Merge functionality to establish relationships between the dimension tables and the fact table via inner joins, using the newly created ID fields for optimal data linkage.

This structured approach ensured a clean, efficient, and well-modeled dataset, ready for insightful reporting and analysis within Power BI.





