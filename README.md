# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
-- SQL - Retail Sales Analysis - p1
CREATE DATABASE sql_project_p1;

DROP TABLE IF EXISTS retail_sales; -- THIS WILL DROP THE TABLE IF IT IS ALREADY THERE
CREATE TABLE retail_sales -- THIS WILL CREATE A NEW TABLE, AND COLUMNS AND THEIR CONSTRAINTS
(
	transactions_id	INT PRIMARY KEY, 
	sale_date DATE, 
	sale_time TIME, 
	customer_id	INT,
	gender VARCHAR(15),
	age	INT,
	category VARCHAR(11),
	quantiy	INT,
	price_per_unit FLOAT,
	cogs FLOAT,
	total_sale FLOAT
)

SELECT * FROM retail_sales;


-- WAQTD/RETRIVE ALL COLUMN FOR SALES MADE ON 2022-11-05
SELECT * FROM RETAIL_SALES
WHERE sale_date = '2022-11-05';

--Write a SQL query to retrieve all transactions where the category is 'Clothing'  
--and the quantity sold is more than 4 in the month of Nov-2022:
SELECT * 
FROM retail_sales
WHERE category = 'Clothing' and  
		TO_CHAR(sale_date, 'YYYY-MM') = '2022-11' and
		quantiy >= 4 ;
--sale_date between 2022-11-01 and 2022-11-30;


--Write a SQL query to calculate the total sales (total_sale) for each category.:
SELECT CATEGORY, SUM(TOTAL_SALE) AS TOTAL_SALE_FOR_EACH_CATEGORY, COUNT(*) AS TOTAL_ORDERS
FROM RETAIL_SALES
GROUP BY 1;

--Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
SELECT ROUND(AVG(AGE),2) AS AVG_AGE
FROM RETAIL_SALES
WHERE CATEGORY = 'Clothing';

--Write a SQL query to find all transactions where the total_sale is greater than 1000.:
SELECT * FROM RETAIL_SALES
WHERE TOTAL_SALE>1000;


--Write a SQL query to find the total number of transactions (transaction_id) 
-- made by each gender in each category.:
SELECT GENDER, CATEGORY, COUNT(*) AS TRANSACTION_MADE
FROM RETAIL_SALES
GROUP BY 1,2
ORDER BY 1;

--Write a SQL query to calculate the average sale for each month. 
--Find out best selling month in each year:
SELECT YEAR, MONTH, AVG_SALE
FROM (
SELECT 
EXTRACT(YEAR FROM SALE_DATE) AS YEAR,
EXTRACT(MONTH FROM SALE_DATE) AS MONTH,
AVG(TOTAL_SALE) AS AVG_SALE,
RANK() OVER (PARTITION BY EXTRACT(YEAR FROM SALE_DATE) ORDER BY AVG(TOTAL_SALE) DESC) AS RANK
FROM RETAIL_SALES
GROUP BY 1,2
) AS T1
WHERE RANK = 1;


--Write a SQL query to find the top 5 customers based on the highest total sales
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5


--Write a SQL query to find the number of unique customers who purchased items from each category.:
SELECT CATEGORY, COUNT(DISTINCT customer_id) AS NUMBER_OF_UNIQUE_CUSTOMER
FROM RETAIL_SALES
GROUP BY 1;

--Write a SQL query to create each shift and number of orders 
--(Example Morning <12, Afternoon Between 12 & 17, Evening >17):
WITH HOURLY_SALE AS (
SELECT *, 
CASE 
WHEN EXTRACT(HOUR FROM SALE_DATE) < 12 THEN 'MORNING'
WHEN EXTRACT(HOUR FROM SALE_DATE) BETWEEN 12 AND 17 THEN 'AFTERNOON'
ELSE 'EVENING'
END AS SHIFT
FROM RETAIL_SALES
)
SELECT SHIFT, COUNT(*) AS TOTAL_ORDERS
FROM HOURLY_SALE
GROUP BY 1;
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Pranit Laware
LinkedIn:https://www.linkedin.com/in/pranitlaware/
This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback feel free to get in touch!

