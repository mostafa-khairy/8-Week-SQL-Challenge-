# Based off the 8 sample customers provided in the sample from the subscriptions table, write a brief description about each customer’s onboarding journey.Try to keep it as short as possible - you may also want to run some sort of join tomake your explanations a bit easier!

```sql
select
	s.customer_id,
	p.plan_id ,
	p.plan_name,
	s.start_date,
	datediff(day,lag(start_date) over(partition by customer_id order by start_date ),start_date)
from 
	plans p
join
	subscriptions s
on
	p.plan_id = s.plan_id
where 
	s.customer_id in (1,2,11,13,15,16,18,19)
  ```
  
  
  ![image](https://user-images.githubusercontent.com/87584678/222017059-4c0622c0-e9b4-474e-a13e-8364e6fde397.png)

