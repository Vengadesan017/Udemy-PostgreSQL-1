# Array
- array[2] means 2 nd value of
- range_type(lower bount, upper bound)
  - range_type : int4RANGE , INT8RANGE, NUMRANGE , TSRANGE , TSTZRANGE , DATERANGE
  - [ --> rangeis close
  - () --> range  is open
  - [( close open
  - [] close close
  - () open open
  - ` select 
      int4range(1,5),
      numrange(1.433,6.123,'[]') `
  - ` select array[1,2,3] , array[12.123::float] `
### operator
```
select array[1,2,3] = array[1,2,3]
select array[1,2,3] < array[1,2,3]
select array[1,2,3] <= array[1,2,3]

-- inclusion operator @> , <@,  &&  a1 is in a1 or not
select array[1,2,3] @> array[1,2,3]   --in
select array[1,2,3] <@ array[1,2,3]    -- in
select array[1,2,3] && array[1,2,3]    -- is overlaped or not

select 2 in (1,2,3)
select 2 not in (1,2,3)
select 2 = all(array[1,2,3])
select 2 = all(array[2,2,2])
select 2 = all(array[2,2,2])
select 2 = any(array[1,2,3])
select 2 <> any(array[1,2,3])


```
### function
```
-- dimension
select array_dims(array[1,2,3])
select array_ndims(array[1,2,3])
select array_length(array[1,2,3])
select array_lower(array[1,2,3])
select array_upper(array[1,2,3])

-- search funtion
select array_position(array[1,2,3,123,12,3],3)   -- first occurrence
select array_positions(array[1,2,3,123,12,3],3)   -- multiple occurrence

-- modifiation
select array[1,2,3] || array[1,2,3]
select array_cat(array[1,2,3], array[1,2,3])
select array_append(array[1,2,3],3)
select array_remove(array[1,2,3],3)
select array_replace(array[1,2,3],3,6)

```
### Conversion
```
select string_to_array('1,2,3,5',',')   - , delimiter
select string_to_array('1,2,3,5',',','5')   - , delimiter - 5 to null



```
###  array to table

```
create table vv (
id serial primary key
phone text array
)

create table vv (
id serial primary key
phone text []    -- no need to set dimention like (3)
)


-- insert use ' or array function
insert into vv (phone)
values
(array(['123123123','123123123'])),
('{1"23123123","123123123"}')

--  select
select phone from vv
select phone[1] from vv
select * from vv where phone[1]='1232123123' 
select * from vv where '1232123123' any (phone)

select unset(phone) from vv  -- create separate 


-- update
update vv set phone[1]  = '123123123'


-- multi dimensional 
create table vv (
id serial primary key
phone text [][]    -- multi dimensions
)
insert into vv (phone)
values
('{23123123,123123123}')


```


### array vs json
- array - easy, less storage, indexing with gin so speed  but allow one data type
- JSON - additional operators , support indexing

# JSON
- it replace the xml. because it is to heavy to transfer
- "name": "string" or number
```
select '{"name":"ven"}'::json
select '{"name":" ven "}'::jsonb   - trims

create table vv
name json

insert into vv (name)
values '{"title":"Book Name",
          "name":"hsadfgsn"}'

select name->'title' from vv   return in ""
select name->>'title' from vv   return in TEXT

update name set name = name || '{"name":"ven"}'
update name set name = name - 'name'

-- print normal table as json
select row_to_json(aa) from aa

-- aggregare
select json_agg(aa) from aa

-- build json array
select json_build_array(1,2,3,4,5,'123')   --  1:2,3:4
select json_build_array('{1,2,3,4,5}',''123123123','123123123','123123123','123123123'')   --  1:'123123123'

select name from vv order by jsonb_array_length(title->''name) desc

-- list keys
select jsonb_object_key(name) from vv   -- return all the key names

-- Existence Operator
select * from vv where name->'title' ? "John"   -- only the text is apply for int use containment

-- containment
select * from vv where name @> '{"id":1}'

select * from vv where name @> '[{"name":"John"}]'          -- start with john   replacement for like
select * from vv
where (name->>'id')::integer in (1,2,3,4,5,6)
where (name->>'id')::integer > 2

```
### indexing
- explain analyze select * from vv where name @> '{"title":"john"}'  -- it takes too mush time
- use GIN Generalised Inverted Index
- the gin store the row_id of each key word
- ` create index id_gin_index_name on vv using GIN(name) `
- but is takes too memory to store
- so
- ` create index id_gin_index_name_cool on vv using GIN(name jsonb_path_ops) `
- also
- ` create index id_gin_index_name_cool on vv using GIN((name->'title') jsonb_path_ops) `
- its take less memory


