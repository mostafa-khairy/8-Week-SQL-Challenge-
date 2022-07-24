# 1 - What is the total amount each customer spent at the restaurant?

''' sql
select s.customer_id, sum(price) as total 
from dannys_diner.sales s
join dannys_diner.menu m
on s.product_id = m.product_id
group by 1 ;
'''
