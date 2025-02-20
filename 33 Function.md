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

-- parameter name
- do not use column name as paramenter
create or replace funtion my_sum(a int ,b int)
returns int as
$$
 select a + b
$$
language sql

create or replace funtion my_sum(p_id int )
returns int as
$$
 select * from vv where id = p_id
$$
language sql
-- use function in api usage
```
