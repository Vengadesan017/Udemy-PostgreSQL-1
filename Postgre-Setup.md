# PostgreSQL Setup

- Download EDB installar from postgre.org/download
- Install the EDB
- Open pgAdmin4
- Login with root(postgres) password to server

# Create user

- Create new user from Login/Groups Role
- ```CREATE ROLE vengadesan WITH
	LOGIN
	NOSUPERUSER
	CREATEDB
	NOCREATEROLE
	INHERIT
	NOREPLICATION
	NOBYPASSRLS
	CONNECTION LIMIT -1
	PASSWORD 'xxxxxx';```

# Create DB

- `CREATE DATABASE "Sample"
    WITH
    OWNER = vengadesan
    ENCODING = 'UTF8'
    LOCALE_PROVIDER = 'libc'
    CONNECTION LIMIT = -1
    IS_TEMPLATE = False;

COMMENT ON DATABASE "Sample"
    IS 'For learning'; `

-1 for unlimite connectivity 

# Create Table
- `DROP TABLE IF EXISTS "public"."regions";
CREATE TABLE "public"."regions" (
    region_id numeric(10,0) NOT NULL,
    region_name character(25)
)
WITH (OIDS=FALSE);`

# Insert Data 
- `DROP TABLE IF EXISTS "public"."regions";
CREATE TABLE "public"."regions" (
    region_id numeric(10,0) NOT NULL,
    region_name character(25)
)
WITH (OIDS=FALSE);`
