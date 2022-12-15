# 1- How many pizzas were ordered?
```sql
select count(*) total_pizza
from clean_customer_orders
```
![image](https://user-images.githubusercontent.com/87584678/207748984-adf42071-810a-436e-929f-d9a3895db3d4.png)

# 2-How many unique customer orders were made?
```sql
select count(distinct(order_id)) distinct_orders
from clean_customer_orders 
```
![image](https://user-images.githubusercontent.com/87584678/207749029-4891ceca-de11-4607-8066-6b6e4814654a.png)

# 3-How many successful orders were delivered by each runner?
```sql
select 
	runner_id, 
	count(order_id) successful_orders
from clean_runner_orders
where cancellation is NULL
group by runner_id
```
![image](https://user-images.githubusercontent.com/87584678/207749061-8ad9a497-2ebb-4faa-8f5f-52b20fa4eae2.png)
