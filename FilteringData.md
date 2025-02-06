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

-- Order of execution from  then where then select then orderby
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
- FETCH {FIRST or LAST} {Count} {Row or Rows} only
- OFFSET start {ROW or ROWS}
