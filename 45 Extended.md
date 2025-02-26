# Extended PostgreSQL
### create own custom index
- idea is create index like ssn : 111-11-0100
- create table with text and normal order by use seq plan to sort so it take so much od time so use custom method
1. create a new class operator
2. use it with say b-tree index

```
--
create or replace function create or replace function fn_fix_ssn(text) return text as
$$
 begin
    return substring($1,8) || replace(substring($1,1,7),'-','')
 end:
$$ language plpgsql immutable;(text) return text as
$$
 begin
    return substring($1,8) || replace(substring($1,1,7),'-','')
 end:
$$ language plpgsql immutable;
--
create or replace function fn_com_ssn(text,text) return int as
$$
 begin
    if fn_fix_ssn($1) < fn_fix_ssn($2) then return -1;
    elseif fn_fix_ssn($1) > fn_fix_ssn($2) then return 1;
    else return 0;
    end if
 end:
$$ language plpgsql immutable;
--

create operator class op_class_ssn_ops1
for type text using btre
as
   operator 1 <,
   operator 2 <=,
   operator 3 =,
   operator 4 >=,
   operator 5 >,
   function 1 fn_ssn_com(text,text);


--
create index inx_ssn on ssn(ssn op_class_ssn_ops1);

--
set enable_seqscan='off';
```

### own aggregate function

```
create aggregate agg_name(p1,p2)
(
  initcond = n,
  stype = type_name,
  sfunc = fun_name,               -- state transistion function to called for eash input row
  finalfunc = final_function_name  -- compute the ahhregate
);

--
create table t_taxi with trip_id , km
create funtion taxi_all_rows(num , num ,num) return $1+$2*$3;
x= taxi_all_rows(initcond = 5 , 3.5 , 2.5)
x= taxi_all_rows(x , 4.5 , 2.5)
x= taxi_all_rows(x , 1.5 , 2.5)
x= taxi_all_rows(x , 3.5 , 2.5)
create function taxi_all_sum(num) return $1;  -- called for each group

create aggregate agg_taxi(numeric,numeric)   --  no of fm , price per km
(
  initcond = 5,
  stype = numeric,
  sfunc = taxi_all_rows,               -- state transistion function to called for eash input row
  finalfunc = taxi_all_sum  -- compute the aggregate
);

select trip_id agg_taxi(km,2.5)
from t_taxi
group by id_trip;
```
