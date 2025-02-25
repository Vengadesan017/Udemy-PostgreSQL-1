# PostgreSQL Setup

- Download EDB installar from postgre.org/download
- Install the EDB
- Open pgAdmin4
- Login with root(postgres) password to server

# Create user

- Create new user from Login/Groups Role
 ```
  CREATE ROLE vengadesan WITH
	LOGIN
	NOSUPERUSER
	CREATEDB
	NOCREATEROLE
	INHERIT
	NOREPLICATION
	NOBYPASSRLS
	CONNECTION LIMIT -1
	PASSWORD 'xxxxxx';
  ```

# Create DB

 ```
  CREATE DATABASE "Sample"
    WITH
    OWNER = vengadesan
    ENCODING = 'UTF8'
    LOCALE_PROVIDER = 'libc'
    CONNECTION LIMIT = -1
    IS_TEMPLATE = False;

COMMENT ON DATABASE "Sample"
    IS 'For learning';
```

- -1 for unlimite connectivity 

# Create Table
```
DROP TABLE IF EXISTS "public"."regions";
CREATE TABLE "public"."regions" (
    r_id SERIAL PRIMARY KEY,
    region_id numeric(10,0) NOT NULL,
    region_name character(25),
    code char(6),
    sub_code Int,
    sub_name  varchar(225) NULL,
    created DATE
)
WITH (OIDS=FALSE);
```
- NUMERIC(precision, scale) (10,2) ==> 12345678.12
  - Precision: Total number of digits.
  - Scale: Number of digits in terms of a fraction.
# Insert Data 
```
BEGIN;
INSERT INTO "public"."regions" VALUES (1,'Europe');
INSERT INTO "public"."regions" VALUES (2,'Americas');
INSERT INTO "public"."regions" VALUES (3,'Asia');
INSERT INTO "public"."regions" VALUES (4,'Middle East and Africa');
COMMIT;


insert into A (a_name) values ('ram');

insert into A (a_name) values 
	('ram'),
	('suresh');

insert into A (a_name) values 
	('ram'),
	('S''suresh');  -- S'suresh

insert into A (a_name) 
	values ('ram') returning *;

insert into A (a_name) 
	values ('ram') returning a_id;
```

# Select Data

```
select * from "public"."regions"
```

# Update data

```
  update A set a_name = 'new name'  where a_id = 1

  update A set a_name = 'new name' , a_sec_column ='New data' where a_id = 1
  
  update A set a_name = 'newww name'  where a_id = 1 returning *

  update A set a_name = 'newww name'  where a_id = 1 returning a_name

  update A set a_name = 'newww name'

  update A set a_name = 'newww name' returning a_name
  ```
  
# Delete data
```
	delete from A where a_id = 1;

	delete from A where a_id = 2 returning a_id;

	delete from A;
```
- Becare full when you delete
# Drop db
- only super user and db user can drop
` drop database if exists vv `
` drop database vv `

# Foreign Key

```
create table A(
	a_id serial primary key,
	a_name varchar(150)
	);

create table B(
	b_id serial primary key,
	b_name varchar(150),
	link_a int references A (a_id)
	);
```
  - It create table constrains (pk, FK , not null, unique , check...)

# Upsert ( Update + Insert )

- When we insert already existing row (at cell) then we will update the row instead of inserting new duplicate
- only in unique column
- ON CONFLICT target action;
  - Target: particular cell in row
  - Action:
    - DO NOTHING
    - DO UPDATE SET column = new_value
```
insert into A (a_no)
values ('ram1')
on conflict (a_no)
do 
	Nothing;



insert into A (a_no)
values ('ram1')
on conflict (a_no)
do 
	update set
		a_no = EXCLUDED.a_no || 1
```
# Alter table 
```
-- add column 
ALTER table A
ADD COLUMN temp varchar(200) NULL

-- rename table
alter table A
rename to aa

-- rename column
alter table aa
rename column temp to extra

-- drop table
alter table aa
drop column extra

-- change data type
alter table aa 
alter column temp type int
using temp::integer

alter table aa 
alter column temp type varchar(10)

-- set default
alter table aa
alter column temp set default 'x'

-- add constraint like  PK , UNIQUE
alter table aa  
add constraint constraint_namee unique (temp)

-- remove Constraint
alter table aa 
drop constraint constraint_namee

-- set a column to accept the only difined allowed values
alter table aa
add column checking varchar(10)

alter table aa
add check (checking in ('Yes','No'))
```

# pgAdmin -propertices
- use pgAdmin to create modify table
- To view table structure , add column
- To rename delete , change data type of column 


# why postgre sql
- fully open source
- community
- create function in sql , plsql , c(very advance)  - build-in
- addtional language pl/py , pl/perl , pl/tcl pl/java    extention needed
### server vs application layer
- best to keep modify the data in server - scalable
- if in app layer then first fetch then modify then updated db server
### function vs procedure 
- function is sql statment , not allow to start or commit , no consistency,   - select fun(if) from xx
- procedure able to control the transaction , even multiple transaction one by one  -- call procedure_name
