Supplier Performance Dashboard
---

<img width="660" alt="image" src="https://github.com/user-attachments/assets/97ba2b6d-8b7f-4a9d-a14e-c6b04a2173c7">

**Table of Content:**
---
* [Problem of Statement](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#problem-of-statement)
* [Data Source](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#data-source)
* [Data Preparation](https://github.com/obdayo/Supplier-Performance-Dashboard/edit/main/README.md#data-preparation)
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

**Data Preparation**
---

Completed the Data transformation in Power Query and the dataset loaded into Microsoft Power BI Desktop for modelling.

The Supplier Quality Dataset Information:

* The Supplier Quality Dataset consist of 9 columns and 5,227 rows for analysis.

Data Cleaning for the dataset was done in the power query editor as follows:

* Created the Supplier Quality Fact table and 6 Dimension tables to model the data.

* Removed irrelevant columns and rows.

* Each of the columns in the table were validated to have the correct data type.

* Removed duplicated values in the dimension tables created.

* Added Index Colunm to each dimension tables, using the numbers as IDs.

* Used Merge queries to merge the IDs on the dimension tables with the Fact table using inner join. 




