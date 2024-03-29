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


# 4- What is the customer count and percentage of customers who have churned rounded to 1 decimal place?
 
```sql
select 
	count( customer_id) count_churn,
	round((count(*) / cast( (select count(distinct customer_id)from subscriptions )as float ))*100,1) as percentage
from 
	subscriptions
where plan_id = 4
```

![image](https://user-images.githubusercontent.com/87584678/222828572-c1213df1-3ff1-4e87-82ec-5a8ceaeb156c.png)

# 5- How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?
 ```sql
with cte as (
	select
		*, 
		ROW_NUMBER() over (partition by customer_id order by start_date) as row
	from 
		subscriptions
			)
select
	count(*) as custmer_churned,
	round(100*count(*)/(select count(distinct customer_id)from subscriptions ),1) as percentage
from
	cte 
where 
	plan_id = 4 and row = 2;

```
![image](https://user-images.githubusercontent.com/87584678/222838276-bf18a54e-d419-4c0d-a9af-a21076cbd940.png)




# 6- What is the number and percentage of customer plans after their initial free trial?
```sql
with cte as (
	select
		p.plan_name,
		ROW_NUMBER() over (partition by customer_id order by start_date) as row
	from 
		plans p
	join 
		subscriptions s
	on s.plan_id = p.plan_id	
	)
select
plan_name,
	count(*) as count ,
	round(100*count(*)/cast( (select count(distinct customer_id)from subscriptions )as float ),2) as percentage
from
	cte 
where 
	row = 2 
group by 
	plan_name
order by
	count	desc;
```

![image](https://user-images.githubusercontent.com/87584678/222914825-0523b644-ae03-4d58-bfc7-748a1494e683.png)




# 7- What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?
```sql
declare @total float =( select count(distinct customer_id) from subscriptions where start_date <= '2020-12-31' ); 

WITH cte AS (
	select 
		p.plan_name,
		ROW_NUMBER() over (partition by customer_id order by start_date DESC) as row
	from 
		subscriptions s 
	join 
		plans p
	on
		s.plan_id = p.plan_id
	where s.start_date <= '2020-12-31'
	)
select 
	plan_name,
	count(*) Total_Customer ,
	count(*)/@total *100 Percentage
from
	cte
where 
	row = 1
group by 
	plan_name
order by 2 desc
```

![image](https://user-images.githubusercontent.com/87584678/222922614-df3548c6-aa7b-413b-a4ca-2af451c6531e.png)


# 8- How many customers have upgraded to an annual plan in 2020?
```sql
select 
	p.plan_name,
	count(*)
from 
	subscriptions s 
join 
	plans p
on
	s.plan_id = p.plan_id
where 
	year(start_date) = 2020
	and
	plan_name = 'pro annual'
group by 
	p.plan_name
```

![image](https://user-images.githubusercontent.com/87584678/222922839-8651cbdd-7275-4f7a-961f-8ce7234c2538.png)



# 9- How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?
```sql 
with plan_annual as (
select 
	* 
from 
	subscriptions
 where 
	plan_id =3
	)

select 
	avg(DATEDIFF(DAY, s.start_date , p.start_date)) as Average_Day
from 
	plan_annual p
join
	subscriptions s
on 
	p.customer_id = s.customer_id
where s.plan_id = 0
```
![image](https://user-images.githubusercontent.com/87584678/222934452-d79df28b-77f7-4e52-966d-2c4d371e6551.png)


# 10 Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)
```sql
with plan_annual as (
select 
	customer_id,
	start_date as annual_date
from 
	subscriptions
 where 
	plan_id =3
	),
plan_trial as (
select 
	customer_id,
	start_date 
from 
	subscriptions
 where 
	plan_id =0
),
bins as( 
select 
	a.customer_id,
    DATEDIFF(DAY, start_date, annual_date)/30 + 1 AS bins
from 
	plan_annual a , plan_trial t
where a.customer_id = t.customer_id	
)
select 
	case when bins = 1 then CONCAT('0 - 30' , ' days')
	else 
	CONCAT( (bins-1)*30 ,' - ',bins*30 , ' days' ) end as breakdown,
	count(customer_id) total_customer
from bins
group by bins
```

# 11 How many customers downgraded from a pro monthly to a basic monthly plan in 2020?
```sql
with Row_Num_CTE as (
select 
	customer_id,
	plan_id,
	lead(plan_id) over(partition by customer_id order by start_date) as row 
from 
	subscriptions
where 
	plan_id=2
)
select 
	count(customer_id) total_Customer
from 
	Row_Num_CTE
where row is not null
```

![image](https://user-images.githubusercontent.com/87584678/222962722-df2914f0-2ec0-4989-812a-cc686482b24d.png)











































































