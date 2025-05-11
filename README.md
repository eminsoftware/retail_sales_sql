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







