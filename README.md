# Retail Sales Analysis PostgreSQL Project

## Project Overview 

**Project Title**: Retail Sales Analysis
**Level**: Beginner

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales table, cleaning it and performing exploratory data analysis (EDA).

## Objectives

1. **Set up a retail sales table**: Create and populate a retail sales table with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to derive insights from the sales data

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `retail_db`.
- **Table Creation**: A table named `retail_tb` is created to store the sales data.

```sql
create database retail_db;

create table retail_tb
(
	transactions_id	int primary key,
	sale_date date,
	sale_time time,
	customer_id	int,
	gender varchar(15),
	age	int,
	category varchar(15),
	quantiy	int,
	price_per_unit float,
	cogs float,
	total_sale float
);
```

### 2. Data Cleaning

- **Null Value Check**: Identifying and deleting null values.

```sql
select * from retail_tb
where 
	transactions_id is null
	or sale_date is null
	or sale_time is null
	or customer_id is null
	or gender is null
	or category is null
	or quantity is null
	or price_per_unit is null
	or cogs is null
	or total_sale is null;

delete from retail_tb
where
	transactions_id is null
	or sale_date is null
	or sale_time is null
	or customer_id is null
	or gender is null
	or category is null
	or quantity is null
	or price_per_unit is null
	or cogs is null
	or total_sale is null;
```

### 3. Exploratory Data Analysis (EDA)


1. **Find the number of sales and the number of customers:**
```sql
select 
	count(*) as total_sales 
from 
	retail_tb;


select 
	count(distinct customer_id) as total_customers 
from 
	retail_tb;
```


2. **Retrieve all columns for sales made on '2022-11-05:**
```sql
select * 
from 
	retail_tb
where 
	sale_date = '2022-11-05';
```

3. **Retrieve all transactions where the category is 'Clothing'
   and the quantity sold is more than or equal to 4 in the month of Nov-2022:**
```sql
select * 
from 
	retail_tb
where 
	category = 'Clothing'
	and 
	to_char(sale_date, 'yyyy-mm') = '2022-11'
	and
	quantity >= 4;
```

4. **Calculate the total sales for each category:**
```sql
select
    category,
    sum(total_sale) as net_sale,
	count(*) as total_orders
from 
	retail_tb
group by 1;
```

5. **Find the average age of customers who purchased items from the 'Beauty' category:**
```sql
select 
	round(avg(age),1) as avg_age
from 
	retail_tb 
where 
	category = 'Beauty';
```

6. **Find all transactions where the total_sale is greater than 1000:**
```sql
select * 
from 
	retail_tb
where 
	total_sale >= 1000;
```

7. **Find the total count of transactions made by each gender in each category:**
```sql
select 
	category, 
	gender,
	count(transactions_id)
from 
	retail_tb
group by 
	1, 2
order by 
	1, 2 desc;
```

8. **Find the best selling month in each year:**
```sql
select 
	year, 
	month, 
	avg_sale
from
	(select 
		extract(year from sale_date) as year,
		extract(month from sale_date) as month,
		round(avg(total_sale)) as avg_sale,
		rank() over(partition by extract(year from sale_date) order by avg(total_sale) desc) as rank
	from retail_tb
	group by 1,2)
where 
	rank = 1;
```

9. **Find the top 5 customers based on the highest total sales**:
```sql
select 
	customer_id,
	sum(total_sale) as total_sales
from 
	retail_tb
group by 1
order by 2 desc
limit 5;
```

10. **Find the number of unique customers who purchased items from each category:**
```sql
select 
	category,
	count(distinct customer_id) as cnt_unique_cs
from 
	retail_tb
group by 1;
```

11. **Create time shifts and number of orders in each shift (Morning <12, Afternoon Between 12 & 17, Evening >17)**
```sql
with hourly_sale
as 
	(select 
		*, 
		case 
			when extract(hour from sale_time) < 12 then 'Morning'
			when extract(hour from sale_time) between 12 and 17 then 'Afternoon'
			else 'Evening'
		end as shift
	from retail_tb)
select 
	shift,
	count(*) as total_orders
from 
	hourly_sale
group by 
	shift;
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.




