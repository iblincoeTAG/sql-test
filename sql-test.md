The SQL provided below should be compatible with `mysql` and so you can use an online compiler like https://paiza.io/en/languages/mysql.

## Question 1:  
Suppose we have a table like this:  
| `category` | `record_id` | `value` |
| :-: | :-: | :-: |
| A | 1 | 0.01 | 
| A | 2 | 0.23 | 
| A | 5 | 0.15 | 
| A | 6 | 0.20 | 
| A | 7 | 0.08 | 
| B | 2 | 1.00 | 
| B | 3 | 0.75 | 
| B | 6 | 0.93 | 
| B | 7 | 0.87 | 

And we want to "fill in the gaps" of the `record_id`s and fill in missing values with the most recent available value. If there is no value available, use the first available value instead. So, the output table should look like:  
| `category` | `record_id` | `value` |
| :-: | :-: | :-: |
| A | 1 | 0.01 | 
| A | 2 | 0.23 | 
| A | 3 | 0.23 | 
| A | 4 | 0.23 | 
| A | 5 | 0.15 | 
| A | 6 | 0.20 | 
| A | 7 | 0.08 | 
| B | 1 | 1.00 | 
| B | 2 | 1.00 | 
| B | 3 | 0.75 | 
| B | 4 | 0.75 | 
| B | 5 | 0.75 | 
| B | 6 | 0.93 | 
| B | 7 | 0.87 | 

Here is a definition of the table in `SQL`, along with a helper table showing all IDs that we want in our output table:  
```sql
with 
	table1 as (
		select 
		    'A' as category, 
		    1 as record_id,
		    0.01 as value union all
		select 'A', 2, 0.23 union all
		select 'A', 5, 0.15 union all
		select 'A', 6, 0.20 union all
		select 'A', 7, 0.08 union all
		select 'B', 2, 1.00 union all
		select 'B', 3, 0.75 union all
		select 'B', 6, 0.93 union all
		select 'B', 7, 0.87
		),
	all_ids as (
	    select 1 as record_id union all
	    select 2 union all
	    select 3 union all
	    select 4 union all
	    select 5 union all
	    select 6 union all
	    select 7
	   )
select * 
from table1;
```  
Can you extend the above query to produce the output table?  

<br/>

## Question 2:

Using the same table set up above, create a new column, `new_var`, which is the `value` variable minus the `value` of the row with the lowest `record_id` for the category. The output table should look like the following:  
| `category` | `record_id` | `value` | `new_var` |
| :-: | :-: | :-: | :-: |
| A | 1 | 0.01 | 0.00 |
| A | 2 | 0.23 | 0.22 | 
| A | 5 | 0.15 | 0.14 | 
| A | 6 | 0.20 | 0.19 | 
| A | 7 | 0.08 | 0.07 | 
| B | 2 | 1.00 | 0.00 | 
| B | 3 | 0.75 | -0.25 | 
| B | 6 | 0.93 | -0.07 | 
| B | 7 | 0.87 | -0.13 |  


<br/>

## Question 3
And lastly, using the same table again, sum up all of the numbers from each `category` splitting out `record_id`s that are even vs. odd. The result should look like this:  
| `category` | `record_id_type` | `value_sum` | 
| :-: | :-: | :-: |
| A | odd | 0.24 |
| A | even | 0.43 |
| B | odd | 1.62 |
| B | even | 1.93 |
