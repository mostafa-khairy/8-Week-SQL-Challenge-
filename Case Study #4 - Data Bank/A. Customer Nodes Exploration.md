# 1- How many unique nodes are there on the Data Bank system?

```sql
select 
	count( distinct node_id ) 'Num unique nodes'
from
	customer_nodes;
```
![image](https://github.com/mostafa-khairy/8-Week-SQL-Challenge-/assets/87584678/bed9aef1-6b51-4ebd-83cf-455b222a51cf)


# 2- What is the number of nodes per region?
```sql
select 
	region_id,
	count( distinct node_id ) 'Num unique nodes'
from
	customer_nodes n 
group by 
	region_id
```
![image](https://github.com/mostafa-khairy/8-Week-SQL-Challenge-/assets/87584678/fd96b664-d9cd-4d13-b511-f7ee65c8f396)

# 3- How many customers are allocated to each region?
```sql
select 
	region_id,
	count( distinct customer_id ) 'Num unique custome'
from
	customer_nodes n 
group by 
	region_id
```
![image](https://github.com/mostafa-khairy/8-Week-SQL-Challenge-/assets/87584678/31caba10-b22b-4263-a32a-f7b95dce8057)

# 4- How many days on average are customers reallocated to a different node?
```sql
with CTE_1 as 
(
select 
	customer_id, node_id,
	DATEDIFF(DAY,start_date,end_date) 'days'
from 
	customer_nodes
WHERE end_date != '9999-12-31'
group by 
	customer_id, node_id, end_date,	start_date
),
CTE_2 AS
(
select 
	customer_id, node_id,
	sum(days) total_days
FROM 
	CTE_1
GROUP BY 
	customer_id, node_id
)
select 
	avg(total_days) VAG_DAY_IN_NODE
from
	CTE_2
```
![image](https://github.com/mostafa-khairy/8-Week-SQL-Challenge-/assets/87584678/226ed5d1-6258-4930-b7b8-8a967a7d245d)


# 5- What is the median, 80th and 95th percentile for this same reallocation days metric for each region?

```sql
with CTE_1 as 
(
select 
	customer_id,region_id, node_id,
	DATEDIFF(DAY,start_date,end_date) 'days'
from 
	customer_nodes
WHERE end_date != '9999-12-31'
group by 
	customer_id, region_id, node_id, end_date,	start_date
),
CTE_2 AS
(
select 
	customer_id, region_id, node_id,
	sum(days) total_days
FROM 
	CTE_1
GROUP BY 
	customer_id,region_id, node_id
)
SELECT 
	distinct region_id, 
	PERCENTILE_CONT(.5) WITHIN GROUP ( ORDER BY total_days )  OVER (partition by region_id) AS 'median' ,
	PERCENTILE_CONT(.8) WITHIN GROUP ( ORDER BY total_days )  OVER (partition by region_id) AS '80th' ,
	PERCENTILE_CONT(.95) WITHIN GROUP ( ORDER BY total_days )  OVER (partition by region_id) AS '95th'
	
from CTE_2
```
![image](https://github.com/mostafa-khairy/8-Week-SQL-Challenge-/assets/87584678/8ef2d687-56a6-4b01-aa6a-c327876fce61)


















































