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













































