# Crosstab Report or pivot table
- to solve the complication of analysis (duplicate, mul row)
- pivot table (inverted table) convert rows to column to easy the perceive, compare , analyze , filter
- it allow agg, sort , organ , reorgan ,grp , sum.. data in db
- by displaying them in table layout / martrix (row,column ,cell)
- need to install ` create extension if not exists  tablefunc; select * from pg_extension;  `

```
select * from crosstab
(
  select- query  -- must 3 column(x,y,z)
) as alias-name
(
  coll type,
  coll type
  ...
)
- y - identifier - row(top th)
- x - categories - column(left side th)
- z - valoue - cell


select * from crosstab
(
  ' select name, subject , score from marks '
) as ct
(
  name varchar,
  English NUMERIC,
  Tamil NUMERIC,
  Maths NUMERIC

)    - name | englist | tamil | maths

select * from crosstab
(
  ' select name, subject , sum(score)::int from marks group by name,subject order by 1, 2  '      
) as ct
(
  name varchar, 
  English NUMERIC,     
  Tamil NUMERIC,       
  Maths NUMERIC

)


-- alaternate method using case
select
  name,
  min(case when subject ='English' then score end) Englis,
  min(case when subject ='Tamil' then score end) Tamil,
  min(case when subject ='Maths' then score end) Maths,
from marks
group by name
or
group by 3

-- alaternate method using filter
select
  name,
  sum(raindays) filter ( where year ='2012') as "2012",
  sum(raindays) filter ( where year ='2013') as "2013",
  sum(raindays) filter ( where year ='2014') as "2014",
from rainfalls
group by location
or
group by 3


```
- cons :from this 3 method output column must be explicitly enumerated, they manually added
 - low flexibility : rewrite the full code for minor update
 - use dynamic pivoting like 1. get column 2. build crosstab 3. execte crosstab
### Dynamic pivot query
- use json_object_agg to create json as single column for multiple column in y
- create function which return the crosstab column
```

select
 location
 json_object_agg(year,total_raindays order by year) as "mydata"
from
(
  select
    location,
    year,
    sum(raindays) as total_raindays
 from
 group by
   location ,
   year

) s
group by location
order by location;



select * from crosstab(
  'master_query'
  'header_column_query'
) as new table
(
 ...
)


create function or replace function pivotcode (
 tablename varchar,
 myrow varchar,
 mycol varchar,
 mycell varchar,
 celldatatype varchar
)
return varchar
language plpgsql as
$$
  declare
    dysql1 varchar,
    dysql2 varchar,
    columnlist varchar
  begin
   -- retrive list of all distinct column name
      dysql1 = select string_agg(distinct ''_''||''mycol||'||'' '||celldatatype||''','','' order by ''_''||'mycol||'||
      execute dysql1 into columnlist;
   -- setup crosstab
     dysql2 = 'select * from crosstab (
      ''select '||myrow||','||mycol||','||mycel||' from ' || tablename |\' group by 1, 2 order by 1,2'',
      ''select distinct ' || mycol||' from '||tablename||' order by 1''
      );';
    -- return the query
      return dysql2
  end
$$

select pivotcode('rainfalls',location','year','sum(raindays)','numeric');


```
- interactive client side pivot in pgsql cmd
- use crosstab( text source_sql , text catrgory_sql) to avoid the mismatching data - crosstab($$..$$ , $$...$$ as CT (columns))
  
