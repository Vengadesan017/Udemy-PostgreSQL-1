# schema
- it is a namespace that contain database obj
- like
  - table
  - view
  - indexes
  - data type
  - functions
  - stored procedure
  - operators
  - hierarchy  : Host > DB > schema > obj name
  - access     : db.schema.table
  ```
  create schema ven
  alter schema ven rename to private
  drop schema private

  -- change schema for tb , view and set scearch path 
  alter table public.aa set schema ven
  select current_schema();
  show search_path          -- username , schema
  set search_path to '$user', vent

  -- change owner
  alter schema ven owner to it
  
  -- duplicate a data as dump in termenal
  pg_dump -d ven -h localhost -u postgre -n public > dump.sql

  -- restore to another schema
  psql - h localhost -U postgres -d private -f dump.sql
  
  
  
  ```
 - system catalog
 - is other then public & user define schema
 - the sys use to the schema
 - it contain sys tables , all build in data , function , operator
 - start with pg_ so reserved
 - ` select * from information_schema.schema: `
 - list the table and column of different table
   ```
   select table_name , column_name from information_schema.columns c

    select table_name , column_name , table_schema 
    from information_schema.columns c
    where c.table_schema = 'public'
    order by 1   
   
   ```
   - grand access to usage
   - ` grand usage on schema schema_name  to username  `
   - ` grand celect ao all tab;es in schema schema_name to user `
