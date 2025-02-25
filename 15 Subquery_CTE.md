# sub query
- Nested inside another query
- Enclosed with ()
- used inside select , insert , update , delete and used after where or from
```
select
* from 
(
	select avg(min_salary) from jobs
) t                 ---alias

-- <
select
* from jobs
where min_salary >
(
	select avg(min_salary) from jobs
)
order by min_salary desc

-- in
select
* from jobs
where min_salary in
(
	select min_salary from jobs where job..
)
order by min_salary desc

-- order without using order by
select
* from jobs
where min_salary in
(
	select first_name, 0 myorder, 'from a' as tablename from a
union
	select first_name, 1, 'from b' as tablename from b
) t
order by myordere

-- without from  but only on data is posible not the full column but multiple column in single row is work like min max()

select
(
	select avg(min_salary) from jobs
) t

-- correlation apply agg in inner and where in outer

-- any
-is like in (= , <> ,< , >
any row with filter
select
* from jobs
where min_salary = any
(
	select avg(min_salary) from jobs where job..
)

-- all
-is like in (= , <> ,< , >
- all row must equal to filter
select
* from jobs
where min_salary = all
(
	select avg(min_salary) from jobs where job..
group by id
)

-- Exist 
- two tables need to = id
select
* from jobs j 
(
	select avg(min_salary) from jobs where history h
  where j.id = h.id

)

```
# CTE - Common Table Expression
- altername for sub query and joins
- cte is temporary result taken from a sql statement in variable but we need to call with base code
- create temp table instead for subquery
- call multiple times in multple place
- non recursive type defaully
- life time of cte is = to life time of query
- with cte_name (c_list) as ( cte_query_definition ) statement
```
with num as 
(
select id ,
(
case
   when id < 10 then 'small'
   else 'lomg'
end
 from generate_series(1,15) as id
)
select * from num


-- logic with pl
with num as 
(
select id ,
(
case
   when id < 10 then 'small'
   else 'lomg'
end
)
 from generate_series(1,15) as id
)
select * from num

-- inset from cte
with num_move as
(
	delete from num
	return *
)
insert into num_table select * from num_muve


-- recursive
- call it selft untile a condition is met

with recursive series (list_num) as
(
	-- non recursive
	select 10 
	union all
	-- recursive
	select list_num + 5 from series where list_num + 5 <= 50
)

with recursive series (list_num) as
(
	-- non recursive
	select * from generate_series(1,15) as id
	union all
	-- recursive
	select list_num + 5 from series where list_num + 5 <= 50
)
select * from series
  ```
