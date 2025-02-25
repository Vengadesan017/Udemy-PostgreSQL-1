# Querying data
### select all the row
- no case sentitive in table name 
```
-- movie DB
select * from A
select a_name from A
select a_name as name from A   -- aliases
select a_name as "A name" from A
```
### select statement
- use || to merge the all teh table output
```
select a_id || a_name from A
select a_id || ' --> ' || a_name from A
select a_id || ' --> ' || a_name as string from A
select 2 * 10 ;
```
### Order by
- To order the result based on table column in ASC and DESC
- asc is default
- can also apply for multiple column but the result come from combinatioon of that orderby condition
- but give priority for first condition
```
select * from A
order by a_id

select * from A
order by a_id desc


select * from A
order by
 a_id desc,
 a_name ASC

-- from hr db test priority
select * from countries
order by 
region_id,
country_name desc;

select * from countries
order by 
country_name desc,
region_id;

-- use alises name in order by
select a_name as name from A
order by name

-- Expressions
select
a_name,
length(a_name)
from A

select
a_name,
length(a_name) as len
from A
order by len desc

-- use column number instead of column name
select a_id, a_name from A
order by
 1 desc,
 2 ASC

-- manage null in order by
select * from A
order by a_id desc
nulls first

select * from A    -- default
order by a_id desc
nulls last            

```

### Distinct values
```
select distinct region_id from countries

select distinct region_id from countries
order by 
region_id

-- return all the values by combining the posible row with distinct value by matching
select distinct region_id , * from countries

select distinct region_id , * from countries
order by 
region_id

select distinct * from countries
order by 
region_id
```

