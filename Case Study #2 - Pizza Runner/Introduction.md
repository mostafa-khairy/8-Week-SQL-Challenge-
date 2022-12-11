# Case Study Questions
## This case study has LOTS of questions - they are broken up by area of focus including:

### Pizza Metrics
### Runner and Customer Experience
### Ingredient Optimisation
### Pricing and Ratings
### Bonus DML Challenges (DML = Data Manipulation Language)

Each of the following case study questions can be answered using a single SQL statement.
Before you start writing your SQL queries however - you might want to investigate the data, you may want to do something with some of those null values and data types in the customer_orders and runner_orders tables!

|Data Cleaning |
|---|

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
