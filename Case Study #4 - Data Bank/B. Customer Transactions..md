# 1- What is the unique count and total amount for each transaction type?
```SQL
select 
	txn_type,
	count(customer_id) 'transaction count',
	sum(txn_amount) 'total amount'
from 
	customer_transactions
group by
	txn_type
```
![image](https://github.com/mostafa-khairy/8-Week-SQL-Challenge-/assets/87584678/08cdcb88-812f-4b89-afd4-9dd6c3fae139)


# 2- What is the average total historical deposit counts and amounts for all customers?
```sql
with total_deposit_each_customer as (
select 
	customer_id,
	count(txn_date) count_deposit,
	sum(txn_amount) total_deposit
from 
	customer_transactions
where 
	txn_type like'%depo%'
group by
	customer_id
)
select 
	AVG(count_deposit)  'average count',
	avg(total_deposit) as 'average amount'
from 
	total_deposit_each_customer 
```

![image](https://github.com/mostafa-khairy/8-Week-SQL-Challenge-/assets/87584678/dab6774b-5dc0-43d1-a23c-7dc3a5b3b9b4)

# 3- For each month - how many Data Bank customers make more than 1 deposit and either 1 purchase or 1 withdrawal in a single month?
```sql
with number_txn_type_for_customer as (
select
	customer_id , month(txn_date) 'months', year(txn_date) 'year',
	count( case when txn_type = 'deposit' then txn_type end ) as 'Number_OF_deposit',
	count( case when txn_type = 'purchase' then txn_type end ) as 'Number_OF_purchase',
	count( case when txn_type = 'withdrawal' then txn_type end ) as 'Number_OF_withdrawal'
from 
	customer_transactions
group by 
	customer_id , month(txn_date) , year(txn_date)
)
select 
	months,
	count(customer_id)	'number_of_customer'
from 
	number_txn_type_for_customer
where 
	Number_OF_deposit > 1 and
	(	Number_OF_purchase >=1 or Number_OF_withdrawal >=1 )
group by 
 months
```
![image](https://github.com/mostafa-khairy/8-Week-SQL-Challenge-/assets/87584678/4ab82fcf-e31b-40b4-b27c-9db00bc2f006)






























































