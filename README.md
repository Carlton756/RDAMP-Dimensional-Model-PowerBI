# RDAMP-Dimensional-Model-PowerBI
This repository showcases my ability to build a structured, query optimized reporting system which incorporates dimensional modeling best practices.
#Objective
As a recap ACE Superstore is a nationawide retail chain which has seen significant sales growth over the period 2023 to the first quarter of 2025. The company is now preparing to expand into additional
regions across the United Kingdom and optimize its current operations. In keeping with the requirement outlined in TASK1, I have prepared a business intelligence report summarizing key sales performance 
trends along with recommendations for improvement.
Going a step further this leg of the project is geared towards designing a demensional model that is usable and scalable. This model should be one that business users can use to analyze performance from mmultiple angles
and in a format that is optimized for enterprise-grade reporting.
# Structure of Project
```
RDAMP-Dimensional-Model-PowerBI/
|__sql/
|  |__create_database.sql
|  |__create_fact_and_dim_tables.sql
|  |__populate_dimension_tables.sql
|  |__populate_fact_table.sql
|  |__create_views.sql
|  |__views_screenshots
|  |__create_reusable_queries.sql
|  |__reusable_queries_screenshots
|__powerbi/
|  |__ACESuperstore_Dashboard.pbix
|  |__dashboard_screenshots/
|  |__schema_diagram.png
|__README.md
```
# Datasets Used for Project
- Carlton_Francis_ACE Superstore Retail_cleaned.csv
- Carlton_Francis_sales staging_cleaned.csv
- Columns used for sales staging table: [Order ID], [Order Date], [Year], [Quarter], [Month], [Month_Number], [Day], [Order Mode], [Customer ID], [City], [Postal Code], [Product ID], [Product Name], [Category], [Sub-Category], [Total_Sales], [Cost Price], [Country],     
                                        [Region], [Gross_Profit_perUnit], [Total_Cost], [Total_Discount], [Total_Revenue], [Total_Units_Sold], [Profit_Margin], [Profit_perUnit], [Segment]
- Carlton_Francis_Items with negative Cost Price_Flagged.csv - I created this file as I believe it would be important to ascertain the reason for the negative values. These values, I believe would affect my analysis.
# 🛠️ Tools Used for Project
## Cleaning tools
-  Excel
## SQL query tools
-  PostgreSQL - For creating sql views and reusable queries
-  VS Code Studio - Connecting to PostgreSQL (This gave me hands on experience in how both VS Code Studio and PostgreSQL can work in tandem as use of both platforms was new to me)
## Business Intelligence tools
-  Power BI - For visualization of intelligence reporting insights through interactive dashboards
# Cleaning Process Using Excel
From the Carlton_Francis_ACE Superstore Retail_cleaned.csv file from TASK1, I created a seperate table (Carlton_Francis_Items with negative Cost Price_Flagged.csv) which contained flagged data. I then removed all the data that made up the flagged dataset from the Carlton_Francis_ACE Superstore Retail_cleaned.csv file to create the Carlton_Francis_sales staging_cleaned.csv file. The data that made up the flagged dataset was made up of rows where Cost Price and Sales had negative values. I took this approach as I believed this would affect my analysis going forward. My appraoch is to ascertain from the business the reason for the negative Cost Price values.
# ⭐ Star Schema Overview
The first step in this project was to create a star schema structure which comprised of the following:
-  Creation of a single sales_fact table which comprised of all the sales metrics that would support the creation of dimension tables
-  Creation 5 dimension tables, which would support our sales_fact table. The dimension tables are as follows:
  -  channel_dim
  -  customer_dim
  -  date_dim
  -  location_dim
  -  product_dim
-  Creation of views tables
## 🌟 Star Schema screenshot
[Star Schema](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/7e47609a72bf00cd780fd3b891a0e3046e0fe3a1/Carlton_Francis_Dimensional%20Modeling%20Star%20Schema.png)
# Schema Explained
| Name of Table | Type | Information |
|---------------|------|-------------|
    


