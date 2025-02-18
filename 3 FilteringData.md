# Types of Operator
- Comparision (=, >, <, <=, >=, <> not equal)
- Logical (AND, OR, LIKE, IN, BETWEEN )
- Arithmatic (+ - / * %)

### where clause - logical
```
select * from A where a_name = 'Ram'

select * from A where a_name = 'Ram' and a_id = 1
select * from A where a_name = 'Ram' or a_id = 1
select * from A where a_name = 'Ram' and (a_id = 1 or a_code = '12')

-- podmase AND then OR
select * from A where a_name = 'Ram' or a_id = 1 and a_code = '12'

-- aliases are not supported in  where but we can use in sub query

-- Order of execution start from  then where then select then orderby

-- in
select * from A where a_name in ('ram', 'ram1)

select * from A where a_name not in ('ram', 'ram1)

-- Between - both are included
select * from A where a_id between 4 and 10
select * from A where a_id not between 4 and 10

-- like and ilike for patterns
-- --  % matchs any sequence from 0 to 2+
-- --  _ for matchs any single character
-- -- like - case sentitive and faster
-- -- ilike - case insentitive and lower
-- like - ~~ , not like !~~
-- ilike - ~~* , not ilike !~~*
select 'hello' like 'h%'   --true ==> %o %e% _ello _hell__ %ll_
select * from A where a_name like 'ram'
select * from A where a_name like 'ram_'
select * from A where a_name like 'ra%'
select * from A where a_name like '____'
```

### where clause - Comparision and date 
```
select * from A where a_id < 5
select * from A where a_id > 5
select * from A where a_id = 5
select * from A where a_id <= 5
select * from A where a_id >= 5
select * from A where a_id <> 5

-- for date use  YYYY-MM-DD or DD-MM-YYYY ...
select * from A where a_date > '2000-01-31'

-- for char
select * from A where a_name < 'ram'
select * from A where a_name < a_code -- column name
```

### limit 
```
select * from A limit 2
select * from A order by a_id desc limit 2
select * from A where a_name = 'ram1' limit 2

-- offset - set where the limit need to start
select * from A limit 2 offset 4
```

### Fetch 
- similar like fetch but had some additional features 
- OFFSET start {ROW or ROWS}
- FETCH {FIRST or Nest} {Count} {Row or Rows} only
- is standard then limit in all rdbms
```
select * from A
fetch first row only

select * from A
fetch first 5 rows only

select * from A
order by a_id
fetch first row only

select * from A
order by a_id
fetch first row only
offset 5

select * from A
order by a_id
offset 5
fetch first row only


select * from A
fetch next 5 rows only
```
### is null and is not null
```
select * from A where a_name is null

select * from A where a_name is not null
```
### Concatenation
```
select a_name || a_id as name from A

select CONCAT(a_name, a_id) as name from A
select CONCAT(a_name,'-->', a_id,a_no) as name from A

select CONCAT_WS('-->',a_name, a_id) as name from A   ---Separator
select CONCAT_WS('-->',a_name, a_id,a_no) as name from A   ---Separator


```
