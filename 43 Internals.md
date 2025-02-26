# Internals
```
-- list all the db users
select * from pg_user;

-- size of db
select dataname, pg_size_pretty(pg_database_size(datname))
from pg_database
order by 2 desc

-- list all db and schemas
select catalog_name from information_schema.information_schema_catalog_name

select * from information_schema.schemata

-- list all table and view
select * from information_schema.tables
select * from information_schema.views


-- list all column
select * from information_schema.columns where table name = 'vv'

-- view system meta data
select current_catalog,current_database(),current_schema,current_user,session_user;

-- current version of server
select version();

-- view all the privileges across db , table ,column
select                                         -- return true of false
has_database_privilege('hr','create'),
has_schema_privilege('public','create'),
has_schema_privilege('public','usage'),
has_table_privilege('movies','insert'),
has_any_column_privilege('movies','select'),
has_column_privilege('postgres','movies','movie_name','select'),
has_column_privilege('vengat','movies','movie_name','select')


--  -- use system administration function
select current_setting('timezone')
select set_config('timezone','EST',true)

-- -file access
-- file if last opn
select pg_stat_file('/etc/aliases')
-- read the file
select pg_read_file('/etc/aliases')    -#
-- list content of directory
select pg_ls_dir('/tmp')


-- show runing query
select * from pg_stat_activity
where query != '<IDLE>' and query not ilike '%pg_state_activity%'
-- how to kill running and idle query
select pg_cancle_backend(12344)  -- 12344 id pid from pg_stat_activity table for running
select pg_terminate_backend(12344)

-- get number of live and dead rows in table and other data like performance monitoring the table
select * from pg_stat_user_tables



```
### file layout of postgresql table
- select data_directory;    -- file path of data directory
- select pg_relation_filepath('vv')   -- file path of table  like 123123
  - 1gb is max size of one file if it increase create second chunck
    - like 3gb of then 3 chunck = 123123 123123.1 123123.2
  - each file is broken up to 8kb chunk called pages
    - in file 123123 it create 131071 pages from page 0 to 131071
    - in 123123.1 = page 131072 to page 196607
  - each row is stored in single page
    - page(TOST) = |hearder |field 1|field 2....|field N|
