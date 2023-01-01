# 1-What are the standard ingredients for each pizza?
```sql
select 
	r.pizza_id,
	pizza_name,
	r.topping_id,
	topping_name
from
	clean_pizza_recipes r, 
	pizza_names n,
	pizza_toppings t
where
	r.pizza_id=n.pizza_id
	and
	r.topping_id=t.topping_id
```
![image](https://user-images.githubusercontent.com/87584678/210181549-d323a9d3-9f61-46b1-b613-c3c7fca4c034.png)

# 2-What was the most commonly added extra?
* first, we need to create session based table to clean data 
```sql
select 
	order_id,
	value extras
into
	#extras
from 
	clean_customer_orders
cross apply string_split(extras,',') 
```
* now we can answer the question 
```sql
select 
	extras as topping_id,
	count(order_id) as count
from 
	#extras
group by
	extras
```	

![image](https://user-images.githubusercontent.com/87584678/210183878-42d8a180-0093-417d-902f-f57fd10522fb.png)


# 3-What was the most common exclusion?

* fist create session based table 
```sql
select 
	order_id,
	value exclusions	
into
	#exclusions
from 
	clean_customer_orders
cross apply string_split(exclusions,',') 
```
* now we can answer the question 
```sql
select 
	exclusions as topping_id,
	count(order_id) as count
from 
	#exclusions
group by
	exclusions
```
![image](https://user-images.githubusercontent.com/87584678/210183968-de0ed196-33f3-48b5-bd19-7e94c060bbf8.png)















































