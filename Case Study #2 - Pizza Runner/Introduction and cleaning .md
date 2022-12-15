# Case Study Questions
## This case study has LOTS of questions - they are broken up by area of focus including:

### 1. Pizza Metrics
### 2. Runner and Customer Experience
### 3. Ingredient Optimisation
### 4. Pricing and Ratings
### 5. Bonus DML Challenges (DML = Data Manipulation Language)

Each of the following case study questions can be answered using a single SQL statement.
Before we start writing SQL queries, however - we might want to investigate the data, we may want to do something with some of those null values and data types in the customer_orders, runner_orders, and pizza_recipes tables!

|Data Cleaning |
|---|
## table customer_orders

- first, we will be dealing with nulls and create a new table with clean data 
- create table by using (select ... into...) to insert data after cleaning

```sql
select
	order_id,
	customer_id,
	pizza_id,
	CASE 
	WHEN exclusions IN (' ' ,'null') then null
	else exclusions
	end as exclusions ,
	case 
	WHEN extras IN (' ' ,'null') then null 
	else extras
	end as extras,
	order_time
into 
  clean_customer_orders
from	
	customer_orders
```
___
## table runner_orders
```sql 
select * from runner_orders
```
```sql
select
	order_id,
	runner_id,

	case
	when pickup_time = 'null' then null 
	else pickup_time
	end as pickup_time,
	
	case 
	when distance = 'null' then NULL
	when distance like '%km%' then trim('km' from distance) 
	else distance
	end as distance,

	case 
	when duration = 'null' then NULL
	when duration like '%minutes%' then trim('minutes' from duration)
	when duration like '%mins%' then trim('mins' from duration)
	when duration like '%minute%' then trim('minute' from duration)
	else duration
	end as duration,

	case 
	WHEN cancellation IN (' ' ,'null') then null 
	else cancellation
	end as cancellation

into 
	clean_runner_orders
from	
	runner_orders
```
```sql
drop table runner_orders
```
cheek data type
```sql
select * from clean_runner_orders
```
```sql
SELECT DATA_TYPE 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE 
     TABLE_NAME   = 'clean_runner_orders' 
```     
```sql
alter table clean_runner_orders alter column  distance float 
```
```sql
alter table clean_runner_orders alter column  duration int 
```
___
## **table pizza_recipes**

```sql
select * from pizza_recipes
```
```sql
SELECT  COLUMN_NAME, DATA_TYPE 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE 
     TABLE_NAME   = 'pizza_recipes' 
```

first, we need to convert the data type for column toppings to varchar() to use the (cross apply string_split) function 
```sql
alter table pizza_recipes alter column toppings varchar(max) 
```sql
select 
	pizza_id,
	value new 
into	
	clean_pizza_recipes
from pizza_recipes 
cross apply string_split(toppings , ',') toppings
```

```sql
drop table pizza_recipes
```
___

## table pizza_name
```sql
alter table pizza_names alter column pizza_name varchar(max)
```

























