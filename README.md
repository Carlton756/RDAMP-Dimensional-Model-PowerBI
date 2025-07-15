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
-  PostgreSQL
-  VS Code Studio
## Business Intelligence tools
-  Power BI



