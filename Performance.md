# Indexing
- index is a structured relation, to access the data in db as faster
- is data structure liek tuple so easy to search the data
- index in particular column till it to be 37
- it can slow the insert update delete (hight cost)
- types
  - index - create an index on only value of columns
  - unique index - create an index on only unique value of columns
```
create index index_name on tb (using method)  method - column name
(
column_name [ASC | DESC} [NULLS {FISRT | LAST}],

-- name for index idx_table_name_column_name
-- name for unique index idx_u_table_name_column_name

create index idx_employees_email on employees (email)
create index idx_employees_email on employees (email,phone)

create index idx_u_employees_email on employees (employee_id)
```
