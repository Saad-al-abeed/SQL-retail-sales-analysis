# Retail Sales Analysis - End-to-End SQL Project

## 📌 Overview

This project demonstrates a comprehensive data analysis workflow using SQL. It covers the entire process from database creation and data cleaning to exploratory data analysis (EDA) and complex query formulation. The goal is to extract actionable business insights from a retail sales dataset, answering specific business questions regarding customer demographics, sales trends, and product performance.

## 🛠️ Technology Stack

* **Language:** SQL
* **Database Environment:** PostgreSQL / MySQL (Utilizing functions like `to_char` and `extract`)
* **Core Concepts Applied:** DDL/DML, Aggregate Functions, Filtering, `GROUP BY`, Common Table Expressions (CTEs), Conditional Logic (`CASE WHEN`), and Date/Time formatting.

## 🗄️ Database Setup & Schema

The project centers around a single table named `retail_sales`.

```sql
-- Creating the database and table
CREATE DATABASE SQL_retail_sales_analysis;
USE SQL_retail_sales_analysis;

CREATE TABLE retail_sales(
    transaction_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);

```

## 🧹 Data Cleaning & Preparation

Before conducting any analysis, the dataset was evaluated for missing or `NULL` values to ensure data integrity. Incomplete records were systematically removed.

```sql
-- Identifying and deleting records with missing data
DELETE FROM retail_sales 
WHERE 
    transaction_id IS NULL OR sale_date IS NULL OR sale_time IS NULL OR
    customer_id IS NULL OR gender IS NULL OR age IS NULL OR
    category IS NULL OR quantity IS NULL OR price_per_unit IS NULL OR
    cogs IS NULL OR total_sale IS NULL;

```

## 📊 Exploratory Data Analysis (EDA)

Basic EDA queries were executed to understand the shape and scope of the dataset:

* **Total Sales:** `SELECT COUNT(*) FROM retail_sales;`
* **Unique Customers:** `SELECT COUNT(DISTINCT customer_id) FROM retail_sales;`
* **Product Categories:** Identified unique categories (e.g., Clothing, Beauty, Electronics).

## 💡 Business Problems & SQL Solutions

The core of the project involves solving 10 specific business questions using SQL.

**Q.1 Write an SQL query to retrieve all columns for sales made on '2022-11-05'.**

```sql
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';

```

**Q.2 Write an SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Nov-2022.**

```sql
SELECT * FROM retail_sales
WHERE category = 'Clothing' 
  AND quantity >= 3 
  AND to_char(sale_date, 'YYYY-MM') = '2022-11';

```

**Q.3 Write an SQL query to calculate the total sales (total_sale) for each category.**

```sql
SELECT category, SUM(total_sale) AS total_sales 
FROM retail_sales 
GROUP BY category;

```

**Q.4 Write an SQL query to find the average age of customers who purchased items from the 'Beauty' category.**

```sql
SELECT category, AVG(age) AS average_age 
FROM retail_sales 
WHERE category = 'Beauty'
GROUP BY category;

```

**Q.5 Write an SQL query to find all transactions where the total_sale is greater than 1000.**

```sql
SELECT * FROM retail_sales WHERE total_sale > 1000;

```

**Q.6 Write an SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**

```sql
SELECT category, gender, COUNT(*) AS total_transactions
FROM retail_sales 
GROUP BY category, gender 
ORDER BY category, gender;

```

**Q.7 Write an SQL query to calculate the average sale for each month. Find out best selling month in each year.**

```sql
SELECT 
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month, 
    AVG(total_sale) AS average_sale
FROM retail_sales
GROUP BY year, month
ORDER BY year ASC, average_sale DESC;

```

**Q.8 Write an SQL query to find the top 5 customers based on the highest total sales.**

```sql
SELECT customer_id, SUM(total_sale) AS total_sales 
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC 
LIMIT 5;

```

**Q.9 Write an SQL query to find the number of unique customers who purchased items from each category.**

```sql
SELECT category, COUNT(DISTINCT customer_id) AS customer_count 
FROM retail_sales
GROUP BY category;

```

**Q.10 Write an SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17).**
*This query utilizes a Common Table Expression (CTE) and a `CASE` statement to categorize transactions based on the time of day.*

```sql
WITH hourly_sale AS (
    SELECT *, 
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift 
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders 
FROM hourly_sale 
GROUP BY shift; 

```

## 💻 Getting Started

1. Clone this repository:
```bash
git clone https://github.com/Saad-al-abeed/retail-sales-analysis-sql.git

```


2. Open the project in Kate or your preferred SQL database management tool.
3. Execute the provided `query_script.sql` file to set up the database schema, insert data, and test the queries.

## 🔮 Future Scope

* **Dashboard Integration:** Connect this robust, cleaned dataset to a visualization tool like Tableau or PowerBI to create interactive, real-time sales dashboards.
* **Backend Development:** Utilize this schema as the foundation for a REST API to serve retail data to a web frontend.

---
