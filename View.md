# View 
- automate the repeated code execution
- stored query to store the long query
- virtual table
- 1. create a view
  2. join to regular table or other view
  3. use the view to update or insert data into table
- create or replace view view_name as query 

```
create or replace view v_jobs_quick as 
select * from jobs
join--
where---
union--

insert into v_jobs_quick (c1,c2) values ('fsdf','sdfsd,)

delete from v_jobs_quick

update v_jobs_quick set c2 = 'ssd' where ..

select * from v_jobs_quick

alter view v_jobs_quick rename to v_new_name

drop view v_jobs_quick


-- check
create or replace view v_jobs_quick as 
select c1,c2 from jobs
where  c2 =' text '
with check option

-- local check
create or replace view v_jobs_quick as 
select c1,c2 from jobs
where  c2 =' text '
with local check option     -- it check the c2 as text only in this view but the data is inserted in main table is not visible in this view

-- cascaded check
create or replace view v_jobs_quick_name as 
select c1,c2 from v_jobs_quick
where  c1 ='name'
with cascaded check option     -- it check the c2 as text and also check the parent view condition


-- can not change the order of column , can not drop the column in replace
-- can add column at the end

-- the aggregate function and set returning are not used in selecting list

-- can use insert , update , delete with where
```
#  materialized view
  - store result of a query physically, then update data periodically
  - cache the result of complex expensive query and then refresh periodically
  - execute the query once and then hold result until refresh
  - create materialized view if not exists veiw name as query with [no] data
  
    ```
    create materialized view if not exists mv_name as 
    view mv_jobs_quick as 
    select c1,c2 from vv
    with no data

    refresh materialized view mv_jobs_quick
    
    drop materialized view mv_jobs_quick
    
    drop materialized view concurrently mv_jobs_quick -- creat temporary updated version of mv, compare two version then perform 
    

    - no insert,update,delete to view mv_jobs_quick only to table


    to check updated or not
    select relispopulated from pg_class where relname = 'mv_jobs_quick2'

    
    ```

