# Managing table
- document every thing
- test in non production server
### select into
``` 
select * into vv_log from vv

select * into vv_log
from
(
  select v.id,v.name,m.mark from vv v
  inner join marks m on m.vid = v.id
  ) as t

- but noe use in pl/pgsql
```
### duplicate a table
```
create table vv_log as (select * from vv) 

create table vv_log as (select * from vv) with no data;
```
### import & export from / to csv file
```
in terminal  import
\copy country_code(country name, iso_code,region)   - first create table
from 'country_iso_code.csv' delimiter ',' csv header;

in terminal  export
\copy country_code to 'country_iso_code_file_name' delimiter ',' csv header;

```
### deleting duplicate data
```
-- 1
select color, count(*)
from colors
group by color
having count(*) >1

delete from colors a using color b  -- self join
where
 a.color_id < b.color_id
 and a.color = b.color

-- 2 for large table
select * from
(
  select colour_id,
  row_number() over(partition by color order by color_id as row
  from colors
) as dups
where
dups.Row > 1 
```

### Database engine operation AND table size
#### table grow for insert, update, delete
  - when you update the value in row the db create new version  of row but it does't delete the old version( as dead rows)
  - when you delete a row , it lives as dead row in the table
  - the db engine use that dead row to multiple transactions where the old version of row is needed
#### to solve the above, the db engine automatically use vacuum to return unused space and much more
  - auto vacuum
    - when large no of row get deleted
    - enable by default
    - no interface
    - by default it run every minute
#### Vacuum command by manual
  - vacuum [full] [freeze] [verbose] table name;
  - analyze table_name [ (c1,c2,c3..)];
  - full
    - Write full content of table into a new file,
    - this reclaims all unused space and requirs an exclusive lock on each table that is vacuum
    - it take time ti execute
    - there is now update occure when full is in running
  - freeze
    - tuplpe are aggressively froven when the table is vacuum
    - default when full is used
    - it redundant both full and freeze
  - verbose
    - an activityreport will be printed detailin the vacuum activity for each table
  - Analyze
    - statistics used by the planner will be updateed
    - used for efficient plan
  - with out full it mark the spaceby dead rows
  - with full create new version of table and discard all dead rows
```
-- check the table size for insert , update , delete
select pg_total_relation_size('vv'),pg_size_pretty(pg_total_relation_size('vv'));
insert into vv select * from generate_series(1,100000);   -- 5688kb
update vv set id =id+1;                                   -- 11mb


-- auto vacuum process
select relname,vacuum_count... from pg_stst_all_tables where relname = 'vv'  -- check the auto vacuum reduce the sizze or not
- but still the memory is not release
- because autovacumm run analysis command to gather data on content of table which is use in query execution plan


-- vacuum command
vacuum verbose vv   - show is 193 unused item, done manual vacuum but same 11mb
vacuum full vv   - done manual vacuum and clear the dead items

```

### Generate columns
- compute column or virtual column
- like view but for column
- altername for trigger but it is elegant and cleaner
- cannot compute on fly at every time
- stored keyword is must
- can't create partition
- copy and pg_dump omitted the column
- explicitly include them like - copy ( select * from vv) to stdout instead of copy t to stdout
```
create table box(
w real,
h real,
area real generated always as (w*h) stored
)
```



