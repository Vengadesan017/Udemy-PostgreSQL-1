# partitioning
- spiltting very large table into multiple smaller , logical diveded pieced table
- increate performance
- based on query run like where clause
- maintain partition as sep table
#### Inheritance
- oop 
- master - > master_child and
```
create table master (id primary key , name varchar(200))

create table master_child() inherits (master)   --  check the pk

when insert,update to child it affect to master but not in child from master
select * from only master
select * from only master_only

drop table master cascade
```
- types of partition: Range(set of column) , list(key values appear) , Hash (specifiying a modules)
- Range
  ```
  create table vv (id primary key , name varchar(20),dob date) partition by range (dob)

  -- create 2 partition
  create table vv_dob_y2000 partition of vv
  for values from ('2000-01-01') to ('2001-01-01')
  
  create table vv_dob_y2001 partition of vv
  for values from ('2001-01-01') to ('2002-01-01')

  insert the data with dob
  select data
  select * from vv   -- all row  - search in all the 3 table
  select * from only vv   -- no row
  select * from only vv_dob_y2000   -- row with 2000
  select * from only vv_dob_y2001   -- row with 2001
  ```
  
- List
  ```
  create table vv (id primary key , name varchar(20),dob date) partition by list (dob)

  -- create 2 partition
  create table vv_country_us partition of vv
  for values in ('US')
  
  create table vv_country_uk partition of vv
  for values in ('UK')

  insert the data with dob
  select data
  select * from vv   -- all row  - search in all the 3 table
  select * from only vv   -- no row
  select * from only vv_country_us   -- row with us
  select * from only vv_country_us   -- row with uk
  ```
- Hash
  - use when logically partition the data
  ```
  create table vv (id primary key , name varchar(20),dob date) partition by hash (id)

  -- create 2 partition
  create table vv_country_1 partition of vv
  for values with (modulus 2, remainder 0)
  
  create table vv_country_2 partition of vv
  for values with (modulus 2, remainder 1)


  insert the data with dob
  select data
  select * from vv   -- all row  - search in all the 3 table
  select * from only vv   -- no row
  select * from only vv_country_1   -- row with us
  select * from only vv_country_2   -- row with uk
  ```
- default
  - if data is not fit in any partition it stored in default
```
  create table vv (id primary key , name varchar(20),dob date) partition by list (dob)

  -- create 2 partition
  create table vv_country_default partition of vv
  default
  
  insert the data with dob
  select data
  select * from vv   -- all row  - search in all the 3 table
  select * from only vv   -- no row
  select * from only vv_country_default   -- row with other the uk and us
  select * from only vv_country_us   -- row with uk
```

### multilevel partition
- sub partition
  ```
  create table vv (id primary key , name varchar(20),dob date) partition by list (dob)

  -- create 2 partition
  create table vv_country_us partition of vv
  for values in ('US')
  
  create table vv_country_uk partition of vv
  for values in ('UK')

  -- create sub partition
  create table vv_country_uk_1 partition of vv_country_uk
  for values wit (madulus 3 , remainder 1)\
  
  insert the data with dob
  select data
  select * from vv   -- all row  - search in all the 3 table
  select * from only vv   -- no row
  select * from only vv_country_uk_1   -- row with uk_1
  select * from only vv_country_us   -- row with uk
  ```
#### altering the bounds of partition
1. detach
2. alter
3. attach
```
begin transaction;
  alter table t1 detqach partition vv_country_us
  alter table t1 attach partition vv_country_us in ('USA')
commit transaction;
```
#### partition index
- create index for each partition
- create index oon vv with id
- then on vv (id , country)
#### pruning on/off
- set enable_partitioon_puring = on -- enable the parition in searching by where
- set enable_partitioon_puring = off -- search data from all the partition
- and enable constraint_exclusion to use particular partition table to search
