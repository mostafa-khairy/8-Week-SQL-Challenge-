# 1- How many customers has Foodie-Fi ever had?
```sql
select 
	count(distinct customer_id) 
from 
	subscriptions
```
![image](https://user-images.githubusercontent.com/87584678/222424636-b6acd066-7455-4c50-b158-bc0234fa2cda.png)


# 2- What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value
 
```sql
select 
	datepart(MONTH,start_date) Month,
	count(s.customer_id) count
from
	plans p 
join
	subscriptions s
on 
	p.plan_id = s.plan_id
where 
	p.plan_name = 'trial'
group by 
	datepart(MONTH,start_date)
```
![image](https://user-images.githubusercontent.com/87584678/222424813-8997f21e-66b3-42db-8ef9-b4e995b6762c.png)


# 3- What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name
 
```sql
select 
	plan_name,
	count(s.customer_id) count
from
	plans p 
join
	subscriptions s
on 
	p.plan_id = s.plan_id
where 
	year(start_date) > 2020
group by 
	plan_name
```
![image](https://user-images.githubusercontent.com/87584678/222424971-62fd5b1c-91a4-4aa7-ac0e-ef895f81d314.png)












































































































