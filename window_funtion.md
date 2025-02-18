# window function
- Like aggregate function but no flattening data (conparing the data into single row)
### over
- use aggregte funtion resulty all rows
- aggregate_funtion over (PARTITION BY group_name)
```
select country, year , export , avg (export) over() as agv_export from trades   -- show avg of all row in all rows

select country, year , export , avg (export) over(partition by country) as agv_export from trades   -- show avg of each country inthere rows

select country, year , export , avg (export) over(partition by year < 2000) as agv_export from trades   -- show avg of each country inthere rows

update trades set import = round(import/10000,2)

select country, year , export , avg (export) over(partition by year order by year) as agv_export from trades   -- this like apply math ops in excel or every row


```
#### sliding windows
- do aggregate function with preceding row and following row
` select country, year , export , avg (export) over(partition by year order by year rows between 1 preceding and 1 following) as agv_export from trades   -- this will compute the avg of 3 row like 1 before and 1 after `

### window frames 
- indicate how many rows around the current row
- rows or range - indicator - 
- between - start and end - 
- unbounded - every thing -
- by default it takes unbounded preceding and current row
- Range is only unbounded
- rows allow all the options
- 
``` 
select * , array_agg(x) over (order by x) from generate_series(1,3) as x   -- default is apply
select * , array_agg(x) over (order by x rows between current row and unbounded following) from generate_series(1,3) as x  
select * , array_agg(x) over (order by x rows between unbounded preceding and  1 following) from generate_series(1,3) as x



select * , array_agg(x) over () as frame_name from generate_series(1,3) as x -- unbounded on both side


```

### window funtions
- add column to result set taht has been calculates ao the fly
```

select country, year , export , avg (export) over(w_f)
from trades
where country = 'usq'
window w_f as (order by year)   - windows funtion contain the windows frame
                                 -- this is equal to rows between unbounded preceding row and current row

select country, year , export , avg (export) over w_f
from trades
where country = 'usq'
window w_f as (order by year rows between unbounded preceding row and current row) 
```

### rank
- give rank based on  window frame
- rank() - allow dupicates
- dense_rank()  - no duplicates
```
select country, year , export , rank() over(order by export desc) as 'rank' from trades 
```

### ntile function
-  split data into equal group based on rank bucket
```
select country, year , export , ntile(4) over(order by export desc) from trades create a 4 group 
```
### lead and lag()
- move line in result
- lead - forward
- lag - backward
```
select country, year , export , lead(export,2) over(order by year desc) from trades   -  point the 2 row from current row
select country, year , export , lag(export,2) over(order by year desc) from trades 
```

### first_value(), nth_value(), last_value()
- show the n th value
```
select country, year , export , first_value(export) over(order by year desc) from trades - show 1 st valu e in all

select country, year , export , last_value(export) over(order by year desc)from trades
- show last value like cursor  using
-- this is equal to rows between unbounded preceding row and current row

select country, year , export , nth_value(export,2) over(order by year desc) from trades  -2 nd row from current rows
```

### row_number()
- return virtual id for each row and it is start from 1
  ` select country, year , export , row_number() over(order by year desc)  from trades  `
  ` select country, year , export , row_number() over(partition by inport order by year desc)  from trades  `
- pagination
```
  select * from (
select e.first_name,e.last_name , d.department 
row_number() over (
partition by d.department_name,
order by e.salary
)
from employee e
inner join department d on d.department_id = e.department_id
) as t 
where row_number between 6 and 10
```

### correlation
- -1 is strong negative , +1 strong positice
` select country, year , export , corr(inport,export)  from trades  `


### windows funtion summary
| scope    |   type |   function   |   Description   |
|--|--|--|--|
| frame    |   row access |   first value   |   value   |
| frame    |   row access |   last value   |   value   |
| frame    |   row access |   nth value   |   value   |
| partition    |   row access |   lead   |   current row after   |
| partition    |   row access |   lag   |   current row before   |
| partition    |   row access |   row_number   |   current row number   |
| partition    |   ranking |   cume_dist   |  cumulative distribution   |
| partition    |   ranking |   dense_rank   |   rank without gap  |
| partition    |   ranking |   ntile   |   crank in n partition   |
| partition    |   ranking |   percent_rank   |   percent rank|
| partition    |   ranking |   rank   |   rank with gaps   |
