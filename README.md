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
| sales_fact    | Fact | Contains sales metrics|
| location_dim  | Dimension | Contains city, country, postal code and region information where each sale was made |
| product_dim   | Dimension | Contains category, product id, product name, segment, sub-category and total cost of product information |
| channel_dim   | Dimension | Contains order mode information, whether the order was made online or in-store |
| customer_dim  | Dimension | Contains customer id information, city, postal code, region and country where sale was made |
| date_dim      | Dimension | Contains date information in the form of short date, day, month, quarter, month_number and year for when order was made |
# Table Creation Process
## Sales Staging Table Creation
I created a staging table (Carlton_Francis_sales staging_cleaned.csv) from the Carlton_Francis_ACE Superstore Retail_cleaned.csv file using excel. I then created the staging table using the following query:
[Staging table creation](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/09e2d283c20c0f9b28d6f981f193e3919f74357e/Carlton_Francis_creation%20of%20sale%20staging%20table%20screenshot/Created%20sales%20staging%20table.png)
```
-- Creating Sales_Fact table
create table Sales_Fact(
Order_ID VARCHAR(255) not null primary key,
Date Date not null,
Product_ID VARCHAR(255) not null,
Customer_ID VARCHAR(255) not null,
Total_Sales Decimal(10,2),
Cost_Price Decimal(10,2),
Gross_Profit_perUnit Decimal(10,2),
Total_Cost Decimal(10,2),
Total_Discount Decimal(10,2),
Total_Revenue Decimal(10,2),
Total_Units_Sold int,
Profit_Margin Decimal(10,2),
Profit_perUnit Decimal(10,2),
Order_Mode VARCHAR(255) not null,
Postal_Code VARCHAR(255) not null,
foreign key (Product_ID) references Product_Dim(Product_ID),
foreign key (Customer_ID) references Customer_Dim(Customer_ID),
foreign key (Date) references Date_Dim(Date),
foreign key (Order_Mode) references Channel_Dim(Order_Mode),
foreign key (Postal_Code) references Location_Dim(Postal_Code)
);
```
I then populated the sale_staging table by using the import feature in PostgreSQL to import the Carlton_Francis_sales staging_cleaned.csv file.
## Dimension Table Creation
I created the following tables within PostgreSQL:
-  product_dim
-  customer_dim
-  location_dim
-  channel_dim
-  date_dim
[Dimension table creation](


