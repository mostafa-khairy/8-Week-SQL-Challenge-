# 1 - What is the total amount each customer spent at the restaurant?

```sql
select s.customer_id,
       sum(price) as total 
from dannys_diner.sales s
join dannys_diner.menu m
on s.product_id = m.product_id
group by 1 ;
```

# 2. How many days has each customer visited the restaurant?
```sql
SELECT s.customer_id,
       COUNT(DISTINCT s.order_date)
FROM dannys_diner.sales s
group by s.customer_id ;
```

# 3. What was the first item from the menu purchased by each customer?
```sql
select customer_id,
       product_name 
from ( 
       select customer_id,
              order_date,
              product_name,
              ROW_NUMBER() over(partition by customer_id order by order_date ) as orderd
       from dannys_diner.sales s
       join dannys_diner.menu m
       on s.product_id = m.product_id 
       ) table_1
where orderd = 1
```

# 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
```sql
select top(1) product_name, 
       COUNT(customer_id) times 
from dannys_diner.sales s,
     dannys_diner.menu m
where m.product_id = s.product_id
group by product_name
order by 2 desc
```

# 5. Which item was the most popular for each customer?
```sql
select customer_id,
       product_name, 
       _count
from 
     (
       select s.customer_id,
	       m.product_name,	
	       count(s.product_id) _count,
	       rank() over(partition by customer_id order by count(s.product_id) desc) as ranks
       from dannys_diner.sales s,
            dannys_diner.menu m
       where s.product_id = m.product_id
       group by s.customer_id , m.product_name
       ) table_1
where ranks = 1
```












































