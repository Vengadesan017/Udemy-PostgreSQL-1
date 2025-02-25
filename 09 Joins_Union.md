# inner join / left /right / full /
```
select h.start_date,j.job_title from job_history h
inner join jobs j
on j.job_id = h.job_id

-- using use a comman column name
select h.start_date,j.job_title from job_history h
inner join jobs j
using(job_id)

-- filter
select h.start_date,j.job_title from job_history h
inner join jobs j
using(job_id)
where j.tiltle = ''

select h.start_date,j.job_title from job_history h
inner join jobs j
using(job_id)
order by 1 desc null last
limit 5

select h.end_date,j.job_title from jobs j
join job_history h
using(job_id)

-- use casting differenct data type in on condition
```

# self join
- compare two columns in same table
```
select h.job_title,j.job_title from jobs h
inner join jobs j
on j.min_salary = h.min_salary
and h.max_salary <> j.max_salary

```
# natural join
- implicit join based on the same column name in joined table
- applied in inner left right join
```
select * from jobs
natural join job_history

select * from jobs
natural left join job_history

select * from jobs
natural right join job_history


```

# cross join
- multiplication
```
select h.job_title,j.job_title from job_histroy h
cross join jobs j
```

### conbining data  
select 
coalesce(h.start_date,j.job_title) as dataa from job_history h
inner join jobs j
on j.job_id = h.job_id

# UNION
- combine the 2 or 3 select query in single out put
- the order and number and data type of columns mudt be same
- remove duplicate
  ```
  select c1 , c2, 't1' as c3 from t1
  union
  select c1, c2, 't2' as c3 from t2
    order bt 1
  ```
# UNION ALL - allow dupicates
- combine the 2 or 3 select query in single out put
- the order and number and data type of columns mudt be same
- allow duplicate
  ```
  select c1 , c2, 't1' as c3 from t1
  union all
  select c1, c2, 't2' as c3 from t2



  -- different number of column with null
    select c1 , c2 from t1
    union
     select c1, null as c2 from t2

  
  ```

# INTERSECT
  - same condition as union
  - but return deffence of 2 table comman rows
```
  select c1 , c2, 't1' as c3 from t1
  intersect 
  select c1, c2, 't2' as c3 from t2
    order bt 1
```
    
# EXCEP 
  - same condition as union
  - but return the common rows
```
  select c1 , c2, 't1' as c3 from t1
  excep 
  select c1, c2, 't2' as c3 from t2

```
    
  
