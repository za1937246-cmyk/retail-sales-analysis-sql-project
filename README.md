# 🛍️ Retail Sales Analysis – SQL Project
## 📘 Project Overview

**Project Title**: Retail Sales Analysis
**Skill Level:** Beginner
**Database Name:** p1_retail_db

This project showcases practical SQL skills that every data analyst needs — from building a retail sales database to cleaning, exploring, and analyzing the data to uncover meaningful insights.
It focuses on using SQL for data cleaning, exploratory data analysis (EDA), and business-driven reporting, making it a perfect starting point for beginners in data analytics.


## 🎯 Project Objectives

- **Database Setup:** Build and populate a retail sales database with the provided data.

- **Data Cleaning:** Detect and remove incomplete or null records.

- **Exploratory Analysis:** Explore the dataset to understand its structure and key variables.

- **Business Insights:** Use SQL queries to answer real-world business questions and discover insights.

# 🧩 Project Structure
## 1. Database Setup

We begin by creating a new database and table to hold the sales data. The table includes details such as transaction ID, customer demographics, product category, quantity sold, price, and total sales value.

CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);

## 2. Data Cleaning & Exploration

We perform initial exploration to understand the dataset’s completeness and characteristics, followed by cleaning operations to remove null or invalid records.
-- Record Count
SELECT COUNT(*) FROM retail_sales;

-- Unique Customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Unique Categories
SELECT DISTINCT category FROM retail_sales;

-- Find Missing Data
SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL 
OR gender IS NULL OR age IS NULL OR category IS NULL 
OR quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Remove Missing Data
DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL 
OR gender IS NULL OR age IS NULL OR category IS NULL 
OR quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

## 3. Data Analysis & Business Queries

Below are the SQL queries used to answer key business questions:

### 1️⃣ Sales made on a specific date

SELECT * 
FROM retail_sales 
WHERE sale_date = '2022-11-05';

### 2️⃣ Clothing sales with quantity > 4 in November 2022

SELECT * 
FROM retail_sales
WHERE category = 'Clothing'
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
AND quantity > 4;

### 3️⃣ Total sales by product category
SELECT 
    category,
    SUM(total_sale) AS total_sales,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

### 4️⃣ Average age of customers who purchased from ‘Beauty’ category

SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

### 5️⃣ Transactions with total sales greater than 1000

SELECT * 
FROM retail_sales
WHERE total_sale > 1000;

### 6️⃣ Transactions by gender within each category

SELECT 
    category,
    gender,
    COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

### 7️⃣ Identify best-selling month in each year

SELECT 
       year,
       month,
       avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) t
WHERE rank = 1;

### 8️⃣ Top 5 customers by total purchase amount

SELECT 
    customer_id,
    SUM(total_sale) AS total_spent
FROM retail_sales
GROUP BY customer_id
ORDER BY total_spent DESC
LIMIT 5;

### 9️⃣ Unique customers per category

SELECT 
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;

### 🔟 Orders by shift (Morning, Afternoon, Evening)

WITH shift_data AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM shift_data
GROUP BY shift;

## 📊 Key Insights

- **Customer Profile:** Sales are spread across various age groups and product categories.

- **High-Value Orders:** Multiple transactions exceeded 1000, highlighting premium customers.

- **Seasonal Trends:** Monthly analysis helps identify top-performing sales months.

- **Top Customers:** The analysis reveals loyal customers contributing most to total revenue.

## 📈 Reports Generated

- **Sales Overview:** Total sales, order counts, and category performance.

- **Trend Report:** Sales performance across time and daily shifts.

- **Customer Insights:** High-spending customers and demographic patterns.

## 🏁 Conclusion

This project demonstrates the core SQL skills needed by a data analyst — database design, cleaning, EDA, and deriving actionable business insights.
Through this analysis, we can identify customer behavior patterns, sales trends, and category performance, supporting data-driven decision-making.
















