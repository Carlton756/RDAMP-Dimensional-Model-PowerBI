# RDAMP-Dimensional-Model-PowerBI
This repository showcases my ability to build a structured, query optimized reporting system which incorporates dimensional modeling best practices.
# Objective
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
The first step in this project was to create a database within PostgreSQL and star schema structure which comprised of the following:
-  Creation of a single sales_fact table which contained all the sales metrics that would support the creation of dimension tables
-  Creation 5 dimension tables, which would support our sales_fact table. The dimension tables are as follows:
  -  channel_dim
  -  customer_dim
  -  date_dim
  -  location_dim
  -  product_dim
-  Creation of views tables

# Database creation
Created a database to house the component of my model using PostgreSQL. See code used to create database:
```
-- creation of database: rdamptask2;

CREATE DATABASE rdamptask2
```
## VS Code Studio connection to PostgreSQL
Throughout this project, I endevoured to use the opportunity to learn how to use VS Code Studio in tandem with PostgresQL. I had prior used VS Code Studio to as a SQL editor. In order to connect VS Code Studio to PostgreSQL I downloaded the following extensions:
-	SQLTools PostgreSQL/Cockroach Driver
-	SQLTools
#### _____________________________________________________________________________________________________________________________________
See screenshot of my established connection between VS Code Studio and PostgreSQL:
-	![image](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/98283d7e12dc478db83f42c9765c6b2324c5e9c7/VS%20Code%20Studio-PostgreSQL%20connection.png)
## 🌟 Star Schema screenshot
-	![Star Schema](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/7e47609a72bf00cd780fd3b891a0e3046e0fe3a1/Carlton_Francis_Dimensional%20Modeling%20Star%20Schema.png)
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

![Staging table creation](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/09e2d283c20c0f9b28d6f981f193e3919f74357e/Carlton_Francis_creation%20of%20sale%20staging%20table%20screenshot/Created%20sales%20staging%20table.png)

I created a staging table in excel (Carlton_Francis_sales staging_cleaned.csv) from the Carlton_Francis_ACE Superstore Retail_cleaned.csv file using excel. I then created the sales_staging table using the following query in PostgreSQL:
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

![Dimension table creation](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/cf1ef2adbaabaa135baac47d940ae2a897ee1cf4/Carlton_Francis_dim%20table%20creation%20screenshot/Creation%20of%20dimension%20tables.png)

See the following codes:
```
--Creating Dimension tables
-- Creating Poduct_Dim table
CREATE TABLE Product_Dim (
    Product_ID VARCHAR(255) PRIMARY KEY,
    Product_Name VARCHAR(255),
    Category VARCHAR(100),
	Sub_Category VARCHAR(100),
	Segment VARCHAR(100),
	Total_Cost DECIMAL(10,2)
	);
-- Creating Customer_Dim table
create table Customer_Dim(
	Customer_ID VARCHAR(255) PRIMARY KEY,
	Country VARCHAR(255),
	Region VARCHAR(255),
	City VARCHAR (255),
	Postal_Code VARCHAR(255)
);
-- Creating Location_Dim table
create table Location_Dim(
	Postal_Code VARCHAR(255) PRIMARY KEY,
	Country VARCHAR(255),
	Region VARCHAR(255),
	City VARCHAR (255)
	);
-- Creating Date_Dim table
create table Date_Dim(
	Date Date primary key,
	Year int,
	Quarter VARCHAR(255),
	Month VARCHAR(255),
	Month_Number int,
	Day int
);
-- Creating Channel_Dim table
create table Channel_Dim(
	Order_Mode VARCHAR(255) primary key
);
```
## Fact Table Creation
I created the following fact table:
-  sales_fact

![Fact table creation](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/05d155d9b43faae49bcc4be65d6970b79ee798e8/Carlton_Francis_Creation%20of%20sales_fact%20table.png)

See the following code:
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
# Table Population
## Sale Fact Table Population
![Sales Fact Table Population](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/2af687316a475c99ab54d5d97c9b717ad6a47eba/Carlton_Francis_population%20of%20fact%20and%20dim%20table%20screenshots/Insert%20into%20sale_fact%20table.png)

See the following code used for population:
```
-- Insertion of data into sales_fact table
insert into sales_fact (
    Order_ID,
    Date,
    Product_ID,
    Customer_ID,
    Total_Sales,
    Cost_Price,
    Gross_Profit_perUnit,
    Total_Cost,
    Total_Discount,
    Total_Revenue,
    Total_Units_Sold,
    Profit_Margin,
    Profit_perUnit,
	Order_Mode,
	Postal_Code
) 
select 
    Order_ID,
    Date,
    Product_ID,
    Customer_ID,
    Total_Sales,
    Cost_Price,
    Gross_Profit_perUnit,
    Total_Cost,
    Total_Discount,
    Total_Revenue,
    Total_Units_Sold,
    Profit_Margin,
    Profit_perUnit,
    Order_Mode,
    Postal_Code
from sales_staging;
select *
from sales_fact;
```
## Dimension Tables Population
-	[Dimension Tables Deduplication and Population Queries](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/tree/ba6ad15eef4d29e151ee9ab79a7d97f782c8c4b9/Carlton_Francis_population%20of%20fact%20and%20dim%20table%20screenshots)
# Views and Reusable Query Creation
## Views Creation
The view tables that were created speaks to the following insights that will be highlighted within the upcoming visualizations:
1.	vw_product_seasonality: Product performance trends over time
2.	vw_discount_impact_analysis: Correlation between discounts and profits
3.	vw_customer_order_patterns: Average order value, frequency, and profit per customer segment
4.	vw_channel_margin_report: Profitability comparison across online vs in-store
5.	vw_region_category_rankings: Rank categories by profit margin per region
-	[Views Creation Queries](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/tree/044d88c611d9b2f36f17d4010ce8b32ae7a0dbfa/Carlton_Francis_created%20views%20screenshots)
#### See the following codes:
```
-- Creating vw_channel_margin_report
create view vw_channel_margin_report as
select 
	channel_dim.order_mode as purchase_channel,
	date_dim.year as order_date,
	sales_fact.profit_perunit as profit,
	sales_fact.profit_margin as profit_margin
from sales_fact
left join channel_dim on sales_fact.order_mode = channel_dim.order_mode
left join date_dim on sales_fact.date = date_dim.date

select *
from vw_channel_margin_report;

-- Creating vw_customer_order_patterns view
create view vw_customer_order_patterns as 
select customer_dim.customer_id, product_dim.segment,
	sales_fact.total_cost as order_value, 
	sales_fact.total_sales as total_sales, 
	sales_fact.total_revenue as total_revenue,
	sales_fact.profit_perunit as profit,
	count(sales_fact.date) as order_frequency
from sales_fact
left join customer_dim on sales_fact.customer_id = customer_dim.customer_id
left join product_dim on sales_fact.product_id = product_dim.product_id
group by customer_dim.customer_id, product_dim.segment, order_value, total_sales, total_revenue, profit;

select *
from vw_customer_order_patterns;

-- creating vw_discount_impact_analysis view
create view vw_discount_impact_analysis as 
SELECT 
    product_dim.product_name,
	product_dim.category,
	product_dim.sub_category,
    	sales_fact.total_discount AS discounts,
	sales_fact.profit_perunit * sales_fact.total_units_sold AS total_profit,
	sales_fact.profit_margin * sales_fact.total_units_sold AS total_profit_margin
FROM sales_fact
LEFT JOIN product_dim ON sales_fact.product_id = product_dim.product_id;

select *
from vw_discount_impact_analysis;

-- creation of product_seasonality view
create view vw_product_seasonality as
select product_dim.product_name, date_dim.year, date_dim.quarter, date_dim.month, date_dim.day, 
		product_dim.total_cost as Cost_Total,
		sales_fact.total_sales as Sales_Total,
		sales_fact.total_revenue as Revenue_Total,
		sales_fact.profit_perunit as Unit_Profit,
		sales_fact.gross_profit_perunit as Unit_Gross_Profit
from sales_fact
join date_dim on sales_fact.date = date_dim.date
join product_dim on sales_fact.product_id = product_dim.product_id

select*
from vw_product_seasonality;

-- Creating vw_region_category_rankings view
create view vw_region_category_rankings as
select 
	product_dim.category,
	location_dim.region,
	sales_fact.profit_margin as profit_margin,
	rank() over (
		partition by location_dim.region 
		order by sales_fact.profit_margin desc
		)as ranking
from sales_fact
left join product_dim on sales_fact.product_id = product_dim.product_id
left join location_dim on sales_fact.postal_code = location_dim.postal_code
where profit_margin is not null
order by ranking asc

select *
from vw_region_category_rankings;
```
## Reusable Queries
This project includes 5 reusable SQL queries (outside of view) that:
-	Joins fact and dimensional tables
-	Returns strategic business insights
### Screenshots of reusable queries
![Overall discount by channel by year by quarter](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/09d171c6a79547c6a7d7652513d6f0e163896e32/Carlton_Francis_reusable%20sql%20queries%20screenshots/Overall%20discount%20by%20channel%20by%20year%20and%20quarter.png)
![Overall discount by channel over the period](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/de2586b397cb1923b6f42f5839746c1deb726dcc/Carlton_Francis_reusable%20sql%20queries%20screenshots/Overall%20discount%20by%20channel%20over%20the%20period.png)
![Overall top 10 revenue generated by product and region](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/58238f73ae1b95b9b609f384fe6db59cf26c3534/Carlton_Francis_reusable%20sql%20queries%20screenshots/Overall%20top%2010%20revenue%20generated%20by%20product%20and%20region.png)
![Total gross profit by channel](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/f1513310d59cafb19af452509389d43111c07a93/Carlton_Francis_reusable%20sql%20queries%20screenshots/Total%20gross%20profit%20by%20channel.png)
![Total orders made by year and quarter](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/0f260d3dd049690b05e0f2bf393a711f09c8085e/Carlton_Francis_reusable%20sql%20queries%20screenshots/Total%20orders%20made%20by%20year%20and%20quarter.png)
# 📊 Power BI Visualization
[See RDAMP-Dimensional-Model-PowerBI-TASK2](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/38fe2500e5cf899c2f977177fb201e70780c7aeb/Carlton_Francis_Dimensional%20Modeling-SQL%20Transformation-Power%20BI-Reporting.pbix)
1.	Connected PostgreSQL to Power BI and imported fact and dimention tables along with views tables - As Power BI is able to get data from many sources, I imported the fact, dimension and views tables by clicking on the PostgreSQL database source under Get data tab and connected Power BI to my PostgrSQL server and the database (rdamptask2 database), I created initially that housed the created tables.
2.	Generated dashboards by utlilizing information from fact and dimension tables via star schema relationship
-	[Explore Power BI Dashboard for Insights](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/7e2da2174c01c2527a463feaf017257776f51243/Carlton_Francis_Dimensional%20Modeling-SQL%20Transformation-Power%20BI-Reporting.pbix)
## Power BI Visualization Screenshots
- ![Product seasonality trends and profit versus discount correlation](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/ba4f3d0851b151af7541e09f7fcaf638592546b9/Carlton_Francis_PowerBI%20Dashboard%20screenshots/Power%20BI%20insight%201.png)
- ![Insights into customer order patterns and order value by channel and segment](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/48c1a42888cb4eb67801edd3686adbbde0a448ff/Carlton_Francis_PowerBI%20Dashboard%20screenshots/Power%20BI%20insight%202.png)
- ![Order count by year and quarter and AOV by channel insights](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/c880b86641adf104636601066e4f4e1da33b65c3/Carlton_Francis_PowerBI%20Dashboard%20screenshots/Power%20BI%20insight%203.png)
- ![Discounts by channel, year, and quareter](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/e8b2726d33e078d16b64ea6934cabc16856ee35f/Carlton_Francis_PowerBI%20Dashboard%20screenshots/Reusable%20query%20insight%201.png)
- ![Top 10 revenue influencers by product and overall gross profit by channel](https://github.com/Carlton756/RDAMP-Dimensional-Model-PowerBI/blob/aef1cf16ff84d93ef3abfe612ec238fefa783c85/Carlton_Francis_PowerBI%20Dashboard%20screenshots/Reusable%20query%20insight%202.png)
# Summary of insights and Recommendations
## Insight 1
- Total profit generated = 2.08M GBP
- Total sales generated = 293.87K GBP
- Total revenue generated = 3.10M GBP
- Profit and discount correlation revealed a steady decline in profit as discount decreased with only one major outlier. The outlier identified was when discount was 0, there was a large value in the profit (201787 GBP) acquired by the company.
- Product and seasonality figures revealed that products such as compact dishwashers, electric bikes and portable solar generators sold very when and generated high revenue and profit gain which increased in 2024 when compared to 2023.
### Recommendations
- The accumulated sum of discounts generated a higher profit value overall than that of the profit acquired as a result of 0 discount. ACE should continue to implement their discount system and even find ways to improve the system as coustomers value discounts.
- Revisit the products that customers purchase the most and see how the discount system can capitalize on those items.
## Insight 2
- East Midlands ranked number 1 in profit margin by region for every category of product with Wales ranked 12th out of the 12 regions for many of the product categories in relation to profit margin
- Food (3596 GBP) ranked 1st in profit margin by category with Grooming (1 GBP) ranked at the bottom
- Kitchen had the highest order frequency at 896 orders, while bicycles had the highest average order value of 3410 GBP
- Customer BY044939 had the highest order frequency of 16 orders
- Customer FN076092 had the highest order value at 17119 GBP with a profit of 540.99 GBP
### Recommendations
- Promote higher margin products like food and bicycles in underperforming regions like Wales
- Seek to combine high order frequency products with high margin products to improve profit figures
- Implement customer appreciation programs that target valuable customers that make frequent purchases
## Insight 3
- There was an increase in the number of products ordered in 2024 (5231 products)compared to 2023 (4356 products) - 20% increase. This trend was also reflected in quarter figures
- I would like to highlight that while cleaning the initial dataset, I removed 42 rows of records that had negative cost price values. This would have drastically affected my analysis. I would venture to ascertain from the business the reason for the negative values and then reincorporate these records for future analysis
- There was an equal split as it relates to average order value across each order channel with a value of 289 GBP
### Recommendations
- Examine areas that contributed to increase in product orders and increase promotional support
- Apply ML techniques to perform predictive algorithms on specific areas of sales. This method would assist ACE in forcasting future growth
- Establish with finace department the cause of the negative cost price values - Root Cause Analysis
- Maintain an optimized and scalable business intelligence model which is able to accommodate reintroduction of the flagged data when and future data















