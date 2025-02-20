# user define function
- we can create with trusted languagea(py) and un trusted language
- we can use sql command inside the function
```
create funtion function_name (p1 type p2 type..)
returns return_type as
begin
  logic
end
language language_name 


create or replace funtion my_sum(int , int)
returns int as
'
 select $1 + $2
'
language sql

call my_sum()
select my_sum(19,29)

' --> $$
create or replace funtion my_sum(int , int)
returns int as
$$
 select $1 + $2
$$
language sql
create or replace funtion my_sum(int , int)
returns int as
$body$
 select $1 + $2
$body$
language sql

-- return void
use void instead of int
then select fn_update()

-- return single value
use real
then in $$ use aggregate
real -- for single value
double precision -- for floating
bigint  --

-- return conposite (multiple column in single row)
1. use table name as return type but it return as set
2. in fn call print as table - select (fn_data()).*;
3. in fn call print as table - select (fn_data()).column_name;
4. in fn call print as table - select column_name((fn_data()));

-- returning multiple row
1. use setof table name as return type but it return as set
2. in fn call print as table - select (fn_data()).*;
3. in fn call print as table - select (fn_data()).column_name;
4. in fn call print as table - select column_name((fn_data()));

-- return as table
1. in fn call print as table - select * from fn_data();

-- return as table by define as table
create or replace funtion my_sum(int , int)
returns table
(
id int,
name varchar(200)
)
 as
$body$
 select id , name from vv      -- column name and order must same from return and function block
$body$
language sql


-- parameter name
- do not use column name as paramenter
create or replace funtion my_sum(a int ,b int)
returns int as
$$
 select a + b
$$
language sql

-- default value in parameter
create or replace funtion fn_sum(a int ,b int default 10)
returns int as
$$
 select a + b
$$
language sql
select fn_sum(1,2);  - 3
select fn_sum(1);  - 11

create or replace funtion my_sum(p_id int )
returns int as
$$
 select * from vv where id = p_id
$$
language sql
-- use function in api call

-- function based view 
create or replace funtion fn_call_view(a int ,b int)
returns int as
$$
 select * from v_students
$$
language sql


-- drop
drop function fn_sum;
drop function if exists fn_sum(arg_list) [cascade|restrict];
arg_list - for method overloading 

```
