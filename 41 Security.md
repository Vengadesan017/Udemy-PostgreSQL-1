# Security
- create role based access to user by group or individual
- Six level
  - Instance Level - user , role db, login, replication
  - Database - Connection, schema..
  - schema - use schema , create obj insed it
  - table - CURD
  - Column - Allow, restricting acces to column
  - Row  -restricting acces to rows
## Instance level
- All superuser access
- createdb
- createrole
- login
- replication
- login using terminal
```
 create role tl nosuperuser nocreatedb noceaterole nologin
 create role vengat superuser createdb ceaterole login password 'password123456'
 psql -U vengat -d hr
 revoke all on database hr from public  -- permission denied
 grand hr to vendat
```
## Database level
- create schema
- connect to db
- create temp table
- add permission -
` grand connect on database hr to vengat ;` 

## Schema level
- create table , function ..
- usage schema
` 
  revoke all on schema public from public
  grand connect on database hr to vengat
  grand usage on schema public to vengat`

## Table level
- select
- insert
- update
- delete
- truncate
- trigger
- reference
```
grand select on all table in schema public to vengat
grand update on table table_name in public schema_name to vengat
```
## Column level
- select from column
- insert
- update
- reference
- no delete
```
grand select (id, name ) on vv to vengat
```
## Row level 
- must enable at table level . not by default
- then deny all
- best but had performance issue because it use where 
```
grand select on table vv to vengat
alter table vv enable rwo level secirity;   -- no data show from select query

create policy p_vv_sales_max_salary_10000 on vv
for select
to vengat
using (max_salary >= 10000)

-- multiple policy are applicable

drop policy p_vv_sales_max_salary_10000 on vv
```

### using current_user with rls (row level security)
``` grand select on table t_user_data to public
 select * from t_user_data
 select current_user
 create policy p_user_t_user_data on t_user_data
  for select
  to vengat
  using (t_user_data >= current_user)
```
- the user should onle view their own data with out using current_user
  - use session variable can be initialized each time a new user tries to see data
  ```
   create policy p_user_t_user_data_user_session on t_user_data
    for all 
    to public
    using (username=current_setting('rls.username'));

    set rls.username = 'vengat';
    selct * from t_user_data;

    drop policy if exists po_name on t_user_data
    alter table t_user_data disable row level security
  ```
### inspection permissions
```
\z vv;   - see privileges and policies
```
### column level encryption
```
create extension pgcryto;
insert into vv (email)
values (pgp_sym_encrypt('a1@gmail.com','longsecretencryptionkey'))

select pgp-sym_decrypt(email::byte,'longsecretencryptionkey') from vv;
```
  
