# 1- How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
```sql
select  
	DATEPART(week, registration_date) AS Week,
	count(runner_id) as count 
from 
	runners
group by DATEPART(week, registration_date)
```
![image](https://user-images.githubusercontent.com/87584678/208390625-e78cc148-0209-4971-a59d-0f934f8512c4.png)

# 2- What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?
```sql
select 
  r.runner_id , 
  AVG(datediff(minute , c.order_time , r.pickup_time )) as time 
from 
	clean_runner_orders r,
	clean_customer_orders c
where r.order_id = c.order_id
group by r.runner_id
```

![image](https://user-images.githubusercontent.com/87584678/208395219-4c2d5199-fe1e-4a3a-b2b1-e3b5a5f4fc92.png)


# 3- Is there any relationship between the number of pizzas and how long the order takes to prepare?
```sql
with table_1 as (
select 
	c.order_id,
	count(pizza_id) as count,
	datediff(minute , c.order_time , r.pickup_time) as time 
from 
	clean_runner_orders r,
	clean_customer_orders c
where
	r.order_id = c.order_id
	and 
	cancellation is null
group by 
	c.order_id,
	order_time,
	pickup_time
)
select
	count,
	avg(time) average_Time
from 
	table_1
group by
	count
```
![image](https://user-images.githubusercontent.com/87584678/210178808-7c04abaf-d916-4df0-b23a-967b807bc76b.png)

# 4- What was the average distance travelled for each customer?
```sql
select	
	customer_id,
	avg(distance)
from
	clean_customer_orders c,
	clean_runner_orders r
where
	r.order_id = c.order_id
	and 
	cancellation is null
group by 
	customer_id
```
![image](https://user-images.githubusercontent.com/87584678/210178935-5f2bcaad-8b89-4f8e-a9c9-199eb770f95a.png)



























