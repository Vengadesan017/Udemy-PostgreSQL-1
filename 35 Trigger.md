# trigger
- user define function invoke automatically whenever event occur
- event : INSERT , UPDATE , DELETE , TRUNCATE
- associate with : table , view , foreign table
- stage
  - before - before the event
  - after - all changes are availble to triger
  - instead - of the event
- if more then 1 triger in table , then it fired in alphabaticall order and exe by alphabaticall order
- can not manuall exe
- no parameter
- type
  - row level - update 10 rows then 10 time trigger called
  - statement level -  only once for each statement
- usage
  - before 
    - insert/update/delete/trun  in row level in table , table and view  in statement level
    - truncate  0 table in statement level
  - after ( same as before )
    - insert/update/delete/trun  in row level in table , table and view  in statement level
    - truncate  0 table in statement level
  - instead 
    - insert/update/delete/trun  in row level in view , 0
    - truncate  0 0
- pros
  - data auditing,logging , replication
  - easy
  - call sp func()
  - allow recursion
- cons  
 - hard to locate need docementation
 - overhead to dml
 - hard to debug
   - do not change in pk, fk, unique key
   - do not update record table where normally read during in transaction
   - do not read data from table where update the same transaction
   - do not aggregate / summarize over the table your updating

```
1. create function using create function
2. bind trigger function to table using create trigger statment

-- syntax
create function tri_function()
  returns trigger
  language plpgsql
as
$$
 begin
 end
$$

create trigger tri_name {before | after } {event}
on table_name
 [for [each] { row|staement } ]
  execute procedure tri_function



-- create trigger function
create function fn_vv_audit_log()
  returns trigger
  language plpgsql
as
$$
 begin
 -- old - row before the update
 -- new - new row that will be update
 if new.name <> old.name then
  insert into vv_audit (id , name, edit_date)
  values ( old.id,old.namenow());
 end if;
 return new
 end
$$


-- bind the trigger function to table
create trigger tri_name
before update
on vv
 for each row
  execute procedure fn_vv_audit_log

-- display trigger variable
 begin
  RAISE NOTICE 'TG_name %', TG_NAME;
  RAISE NOTICE '==> %', TG_RELNAME;
  RAISE NOTICE '==> %', TG_TABLE_SCHEMA;
  RAISE NOTICE '==> %', TG_TABLE_NAME;
  RAISE NOTICE '==> %', TG_WHEN;
  RAISE NOTICE '==> %', TG_LEVEL;
  RAISE NOTICE '==> %', TG_OP;
  RAISE NOTICE '==> %', TG_NARGS;
 end if;
 return new
 end

-- disallowing delete
 begin
  if TG_WHEN = 'AFTER' THEN   
      RAISE EXCEPTION 'not allow to % rows in %.%', TG_OP,TG_TABLE_SCHEMA,TG_TABLE_NAME;
 end if;
      RAISE NOTICE 'DO NOT allow to % rows in %.%', TG_OP,TG_TABLE_SCHEMA,TG_TABLE_NAME;  -- if before in trigger it also stop the trigger
 return NULL  --*****
 end

create trigger tri_name
before delete
on vv
 for each row
  execute procedure fn_vv_audit_log


-- disallowing truncate
 begin
  if TG_WHEN = 'AFTER' THEN   
      RAISE EXCEPTION 'not allow to % rows in %.%', TG_OP,TG_TABLE_SCHEMA,TG_TABLE_NAME;
 end if;
      RAISE NOTICE 'DO NOT allow to % rows in %.%', TG_OP,TG_TABLE_SCHEMA,TG_TABLE_NAME;  -- if before in trigger it also stop the trigger
 return NULL  --*****
 end

create trigger tri_name
after truncate
on vv
 for each row
  execute procedure fn_vv_audit_log

-- complete loging logic
declare
 old_row json = null;
 new_row json = null;
 begin

  if TG_OP IN ('update','delete') THEN   
      old_row = row_to_json(old);
 end if;

  if TG_OP IN ('insert','delete') THEN   
      new_row = row_to_json(new);
 end if;

 insert into audit_log(username,add_time,table,ops,row_before,row_after)
 value (session_user,current_timmestamp at time zone 'UTC',TG_table_schema || '.'|| TG_TABLE_NAME,
        TG_OP,old_row,new_row)

 return new 
 end

create trigger tri_name
after insert or update or delete
on vv
 for each row
  execute procedure fn_vv_audit_log


```
### conditional trigger
- using generic when clause
- some condition in when clause except subquery
- return boolean . if value is false or null then return false then trigger function is not called
1. when condition
2. use for eaach statement
3. pass parameter to exe procedure function
```
begin
 raise exception '%', TG_ARGV(0);
 return null;
end;

create trigger tri_name
before insert or update or delete or truncate
on mytask
 for each row
  when (
    extract('DOM' from current_timestamp) = 5 __ friday
    and current_time > '12:00'
  )
  execute procedure fn_cancle _mytask_with_message('no update aare allowed at friday afternoon');


-- disallow in PK

create trigger tri_name
after update of id
on mytask
 for each row
  when (
    extract('DOM' from current_timestamp) = 5 __ friday
    and current_time > '12:00'
  )
  execute procedure fn_cancle _mytask_with_message('no update aare allowed at friday afternoon');

```
# event trigger
- data specific and not to bind
- it also capture the ddl
- before and after
- use any language
- disable in single user mode and can only be created by super user
- also conditional
- usage
  - cascading a ddl
  - preventing changes to table
  - limit ddl
  - enable disable ddl
- conditions
   - we must had the function to exe
   - function return specific type called event_trigger
- trigger events
  - ddl_command_start - event occur just before a create , alter or drop ddl command is executed
  - ddl_command_end - event occur just after a create , alter or drop  command has finished executed
  - table_rewrite - event occur just before a table is rewritten by some action of commands  -- alter table and alter type
  - sql_drop  - just before the ddl_command_end event for command that drop db obj
- trigger variable
   - TG_tag  - tags like create drop alter table..
   - TG_event - conatin event name like ddl_command_start
```
-- audit trial event trigger
create or replace functioon fn_event_audit_ddl()
return EVENT_TRIGGER
language plpgsql
security definer
as
$$
 begin
  insert in to audit_ddl ( username, ddl_event,ddl_command, ddl_time)
  value (seesion_user, TG_EVENT, TG_TAG,nox())

 raise notice ' DDL activity is added'!

  -- no return .. if needed
 end;
$$

create event trigger trg_event_audit_ddl
on ddl_command_start
execute procedure fn_event_audit_ddl()

create event trigger trg_event_audit_ddl
on ddl_command_start
when
  tag in ('create table')
execute procedure fn_event_audit_ddl()




-- prevent schema changes
declare
  current_hour int = extract('hour' from now();
 begin
  if current_hour between between 9 and 16 then
    raise exception 'not allow in night'
  end if;

  -- no return .. if needed
 end;

create event trigger trg_event_audit_ddl
on ddl_command_start
execute procedure fn_event_audit_ddl()

create event trigger trg_event_audit_ddl
on ddl_command_start
when
  tag in ('create table')
execute procedure fn_event_audit_d

--
drop event trigger vv_ddl;
```


