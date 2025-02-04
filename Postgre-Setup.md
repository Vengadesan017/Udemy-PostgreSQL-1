# PostgreSQL Setup

- Download EDB installar from postgre.org/download
- Install the EDB
- Open pgAdmin4
- Login with root(postgres) password to server

# Create user

- Create new user from Login/Groups Role
- `CREATE ROLE vengadesan WITH
	LOGIN
	NOSUPERUSER
	CREATEDB
	NOCREATEROLE
	INHERIT
	NOREPLICATION
	NOBYPASSRLS
	CONNECTION LIMIT -1
	PASSWORD 'xxxxxx';`
