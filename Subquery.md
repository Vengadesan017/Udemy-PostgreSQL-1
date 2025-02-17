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
### 
