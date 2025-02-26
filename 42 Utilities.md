# PSQL command line
```
psql -d hr -U vengat    -- local server connection
psql -h 192.23.23.1 -d hr -U vengat    -- remote server connection

\c hr  -- change db
\c hr vengat -- change db
\q  -- quit the connect

\l  -- list of all db with all table must login as postgres user
\dt  --list all table inside the db
\db   --list all table
\dn   -- list available schemas
\di    -- list all indices like vv_pkey with schema name , type , owner table

\ds  -- list all sequences
\dg  -- list all roles
\dT  -- list all data type
\dD  -- list all domains data types

\d vv   -- describe the vv table

\e    -- open vim editor to edit the previous query like appply where filter limit

\g   -- type previous command
\s   -- full history
\i filename  -- run command from the file

\h  -- help - get all commands
\h create
\h create table

\pset null (null)   -- to show null in cmd   -- because the null and empty is show as empty
\pset null   -- unset the changes
\pset linestyle unicode
\pset linestyle ascii
\pset border 2
\pset border 1


\watch 2   -- it run the privious command for every 2 second
 
\timing    -- show timing for query take to run
```
