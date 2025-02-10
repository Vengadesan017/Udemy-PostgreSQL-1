# Data types

### Boolean
- TRUE 'true' 't' 'y' 'yes' '1'
- FALSE 'false' 'f' 'n' 'no' '0'
- NULL

```
create table boolean_tb (
 boolea_id serial primary key,
 is_valid boolean
)

insert into boolean_tb (is_valid) values (TRUE);
insert into boolean_tb (is_valid) values (false);
insert into boolean_tb (is_valid) values ('t');
insert into boolean_tb (is_valid) values ('n');
insert into boolean_tb (is_valid) values ('yes');
insert into boolean_tb (is_valid) values ('0');

select * from boolean_tb;                        -- all values are stored as true or false
select * from boolean_tb where is_valid = true;
select * from boolean_tb where is_valid = TRUE;
select * from boolean_tb where is_valid = 'y';
select * from boolean_tb where is_valid = 'no';
select * from boolean_tb where is_valid = '0';
select * from boolean_tb where is_valid;    ---- only true
```

### Characters 
- may be a text, number, sysmbols
- CHARACTER(n),CHAR(n)    - fixed-length, blank padding  -deffault 1  - if u insert hai in n=5 the pg stores as 'hai  '
- CHARACTER VARYING(n), VARCHAR   - variable- length with length limit - default n is 1
- TEXT , VARCHAR                       - variable unlimited length - upto 1GB
```
select cast("Hai" as character(5)) as name   -- "hai  "
select "Hai"::char(5) as name   -- "hai  "

select 'Hai'::varchar(5) as name   -- "hai"
select 'Hai'::varchar(1) as name   -- "h"

select 'Hadfsdsdfsdffddssdfsdfsdsdfi'::varchar as name
select 'Hadfsdsdfsdffddssdfsdfsdsdfi'::text as name
```

### Numbers
- Numbers but not null
- Integers   -- Whole number both +ve , -ve (auto increment : serial)
  - smallint -2 bytes
  - integer  - 4 bytes
  - bigint   - 8 bytes
- Decimat    -- Two format of fraction of whole number
  - fixed-point 
     - numeric(precision, scale) (5,2---> 123.45)   .9 ---> .90 -- if high cut and round if low fill with 0
  - Floating-point
    - real  - precision upto 6 decimal     -- if high cut and round if low do nothing
    - double  - precision upto 15 decimal   -- if high cut and round if low do nothing

### Date /  time
- Date - only date low 4713 BC high 294276 AD   - YYYY-MM-DD - CURRENT_DATE
- Time - only Time low 4713 BC high 5874897 AD   - HH:MM:SS , HH:MM , HHMMSS, MM:SS.PPPPPp , HH:MM:SS.pppppp , HHMMSS.ppppp (p-precision)
- Timestamp - date and time low 4713 BC high 294276 AD  use (UTC time)
- Timestamptz - data, time , timestamp low 4713 BC high 294276 AD  (time zone) (GMT) (Greenwich Mean Time (GMT) has no offset from Coordinated Universal Time (UTC))
- Interval store value
```
create table table_dates (
id serial primary key ,
add_date DATE not null
add_date_1 DATE CURRENT_DATE
add_time TIME not null
add_time_1 TIME CURRENT_TIME or CURRENT_TIME(2)or LOCALTIME(4)

show TIMEZONE
SET TIMEZONE = 'Asia/Kolkata'
select CURRENT_TIMESTAMP
select TIMEOFDAY()
```

### UUID Universal Unique Identifier
- 128 bit - 32 digital - hexadecimal digita separated by hyphens
- pg did not the inbuild so we need to use thired party like uuid-ossp as extension
  ```
  create extension if not exists "uuid-ossp"

  select uuid_generate_v1()   -- use mac address and  time stamp
  select uuid_generate_v4()  -purly random number


  create table table_dates (
  id UUID DEFAULT uuid_generate_v1(),
  add_date varchar(255) null
  )
  ```

  ### Array
  - add [] next to the data type  ( phone_number text [] )
  - insert into tb (phone_number)  values(array [ '123-123-1232', '123-345-5432' ])

### hstore - key value pair
- need to install as extension
- create extension is not exist hstore
- create table tb (book_info) hstore
- insert into tb (book_info) vlues ( 'Title','"date" => "abc", "cost" => "5000"' )
- select * from tb
- select book_info -> 'date' as "date" from tb

### JSON and JSONB
- insert a data like '[12,132,42]' , '{"key":"value"}' into json
- select * from tb
- insert a data like '[12,132,42]' , '{"key":"value"}' into jsonb (store as binary so easy to index , search)
- select using -> , @>

### Network address
- cidr  - 7 to 19 bytes  - ipv4 and ipv6 newwork
- inet  - 7 to 19 bytes  - ipv4 and ipv6 host and network
- macaddr  - 6bytes  - Mac address
- macaddr8  - 8 bytes  - Mac address (EUI- 64 format)

# User defined data type
### DOMAIN
- create domain nmae as data_type constraint
- unique within a schema scope , can not reuse out side the scope but can reuse in multiple column
```
create domain ven varchar(100) null

create table user_data_type (
id serial primary key,
name ven

insert into user_data_type (name)
values ('haii')
)

create domain ven1 int not null check (Value > 10)
create domain email varchar(150) null check (value ~* '^[A-Za-z0-9]+@[.][a-z]+$')


--- Enum type
create domain color varchar(100) null check(values in ('R' , 'B', 'G'))

--- drop domain
drop domain ven  -- error 

drop domain ven cascade

```
### TYPE : composite data type
- multiple data in single column
```
create type ventype as (
fname varchar(255),
lname varchar(255)
)

create table vent (
id serial primary key,
ventype ventype
)

insert into vent (ventype) values (row('First name','Last name'))

select * from vent


select (ventype).fname from vent

select (vent.ventype).fname from vent


-- enum
create type currency as enum ('USD','INR')
select 'USD'::currency

create table vent (
id serial primary key,
currency currency
)

insert into vent (currency) values ('INR')

-- drop
drop type currency


-- alter
alter type currency rename to all_currency

alter type currency owner to admin

alter type currency set schema private

alter type public.ventype add attibute name narvhar(255)


-- default in enum

create table dd (
id serial,
ddd currency default 'USD'
```





# sequence 
- to find current sequence no. and move to next
  ``` 
  create sequence if not exists test_seq
  create sequence test_seq
  
  select nextval('test_seq')  --iterate to next 
  
  select currval('test_seq')   -- show current value
  
  select setval('test_seq',100)    -- we can set or reset but default it takes as 1 only after using the nextval
  
  select setval('test_seq',100,false)    -- reset to new value when nextval is used
  
  select setval('test_seq',100,true)    -- reset to new value when setval applied
  
  create sequence if not exists text_seq start with 200
  
  alter sequence test_seq restart with 200
  
  alter sequence test_seq rename to new_seq
  
  -- multiple parameter
  create sequence if not exists test_seq
  increment 10
  minvalue 500
  maxvalue 600
  start with 550
  cycle                  -- descening

  --- data type
  create sequence if not exists test_seq as SMALLINT
  
  drop sequence test_seq

  -- add to table
  - create table without serial
  - create sequence with start with 100 oqned by users2.user2_id
  alter table tb_name alter column c1 set default nextval(test_seq)

  -- share 1 seq to 2 table
  - create 1 seq
  - set default nextval('seq') in two columns

  - set default ('id' || nextval('seq')) in two columns
  ```



