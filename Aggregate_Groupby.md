# Aggregate Funtion
### count
- select count(*) from aa
- select count(1) from aa  -- both are same
- select count(a_no) from aa
- select count(distinct(a_no)) from aa

### sum
- select sum(a_id) from aa 
- select sum(distinct(a_id)) from aa
### MIN / MAX
- select min(a_id) from aa
- select max(a_id) from aa
### greatest / least
- select greatest(21,23,123,12)
- select *,greatest(a_name,a_no) from aa
- select *,least(a_name,a_no) from aa
### avg
- select avg(a_id) from aa
### use maths to combiine the column
- select 2+2
- select a_name, (a_id - a_id) as name from aa  - only for numaric column
### round
-  SELECT ROUND(235.415, 0) AS RoundValue; 
### filter 
- selectively pass data to aggregates
```
select job_title,avg(min_salary) filter (where min_salary > 5000) from jobs
group by job_title
select job_title,avg(min_salary) filter (where max_salary > 5000) from jobs

group by job_title
 ```
# Group by and having(distinct,order by and limit)
```
select min_salary,count(min_salary) from jobs group by 1

select min_salary,count(min_salary) from jobs group by 1 order by 2 desc

select min_salary,count(min_salary),sum(min_salary) from jobs group by 1 order by 2 desc

select min_salary,count(min_salary),sum(min_salary),min(min_salary) from jobs group by 1 order by max(min_salary) desc

```
### rollup in group by
- when use grp by with aggregte funtion with two column in grp by
- becuse if 2 column in grp by it not give the result based on grp
- but in roll up it give result like grand total , and eaxh column total and conbination tptal when use sum()
```
select course_level,course_name,sum(sold_unit) from courses
group by rollup(course_level,course_name)
```
- grouping in aggregate
- return 0 or 1
- if null return 1 else 0
```
select course_level,course_name,sum(sold_unit),grouping(course_name),grouping(course_level) from courses
group by rollup(course_level,course_name)

select
Coalesce(course_level,''),
course_name,sum(sold_unit),
case
  when grouping(course_level) = 1 then 'Grand Total'
grouping(course_name),
grouping(course_level)
from courses
group by rollup(course_level,course_name)


```
### cube in group by
- create all possiblity of conbination on columns
- cube(c1,c2,c3) => (),(c1)...,(c1,c2)...,(c1,c2,c3)
```
select course_level,course_name,sum(sold_unit),grouping(course_name),grouping(course_level) from courses
group by cube(course_level,course_name)
```

### grouping set
- create conbination on columns in you given order
```
select course_level,course_name,sum(sold_unit) from courses
group by
grouping sets((),course_level,course_name)
```

# Having
- used with group or aggregate, for checking the condition
```
select min_salary,count(min_salary),sum(min_salary) from jobs
group by 1
having sum(min_salary) < 10000
order by 3 desc 


handling the null
- coalesce(source,'')

select coalesce(min_salary,'no salary mentioned'),count(min_salary),sum(min_salary) from jobs
group by 1
having sum(min_salary) < 10000
order by 3 desc 
```

