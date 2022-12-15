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

# 4-How many of each type of pizza was delivered?
```sql
select 
	p.pizza_name, 
	p.pizza_id,
	count(c.pizza_id) successful_orders
from 
	clean_runner_orders r
	join clean_customer_orders c
	on r.order_id = c.order_id
	join pizza_names p
	on c.pizza_id = p.pizza_id
where 
	r.cancellation is NULL 
group by p.pizza_name,p.pizza_id
```
![image](https://user-images.githubusercontent.com/87584678/207761051-064a227d-649f-48c9-9e95-4a35c1ada2b5.png)


# 5-How many Vegetarian and Meatlovers were ordered by each customer?
```sql
select 
	c.customer_id,
	p.pizza_name,
	count(c.order_id) successful_pizza 
from 
	clean_runner_orders r
	join clean_customer_orders c
	on r.order_id = c.order_id
	join pizza_names p
	on c.pizza_id = p.pizza_id
where 
	r.cancellation is NULL 
group by c.customer_id, p.pizza_name
```
![image](https://user-images.githubusercontent.com/87584678/207761089-4d484383-7968-4cfc-b64b-f70645c59c73.png)


 # 6-What was the maximum number of pizzas delivered in a single order?
```sql
select
	top(1)
	c.order_id,
	count(pizza_id) MaxNumber_OfPizza
from 
	clean_customer_orders c,
	clean_runner_orders r
where 
	r.order_id = c.order_id
	and
	r.cancellation is NULL
group by c.order_id
order by 2 desc
```

![image](https://user-images.githubusercontent.com/87584678/207761138-2c37ae1c-4c7e-4ed4-9a83-8424356031e0.png)


# 7-For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
```sql
select
	c.customer_id,
	count(c.pizza_id) total_delivered_pizza ,
	sum(case 
		when c.exclusions is not null or c.extras is not null then 1  
		else 0
		end) as have_changed,
	sum(case 
		when c.exclusions is null and c.extras is null then 1  
		else 0
		end) as have_no_changed
from 
	clean_customer_orders c,
	clean_runner_orders r
where 
	r.order_id = c.order_id
	and 
	r.cancellation is null
group by c.customer_id
```
![image](https://user-images.githubusercontent.com/87584678/207761195-8f328254-dfc0-4364-ad08-05f1606f4930.png)



# 8-How many pizzas were delivered that had both exclusions and extras?
```sql
select
	sum(case 
		when c.exclusions is not null and c.extras is not  null then 1  
		else 0
		end) as ExclusionsAndExtras
from 
	clean_customer_orders c,
	clean_runner_orders r
where 
	r.order_id = c.order_id
	and 
	r.cancellation is null
```
![image](https://user-images.githubusercontent.com/87584678/207761276-7293fdd7-afd9-4e85-84c7-f5a6a123e3b5.png)


#9-What was the total volume of pizzas ordered for each hour of the day?
```sql
select 
	datepart(HOUR,order_time) _Hour,
	count(order_id) NumberOfOrder
from clean_customer_orders
group by 
	datepart(HOUR,order_time)
```
![image](https://user-images.githubusercontent.com/87584678/207761310-f6f209dc-2da0-44b6-8289-f43ff99afd90.png)


# 10-What was the volume of orders for each day of the week?
```sql
select 
	format(order_time,'dddd') _day ,
	count(order_id) NumberOfOrder
from clean_customer_orders
group by 
	format(order_time,'dddd') 
```
![image](https://user-images.githubusercontent.com/87584678/207761357-76beac54-e08c-4166-830c-192fbeb4adbf.png)

