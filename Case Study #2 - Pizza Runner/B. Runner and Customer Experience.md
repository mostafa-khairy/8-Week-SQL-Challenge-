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




























