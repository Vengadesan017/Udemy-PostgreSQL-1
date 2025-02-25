# Indexing
- index is a structured relation, to access the data in db as faster
- is data structure liek tuple so easy to search the data
- index in particular column till it to be 37
- it can slow the insert update delete (hight cost)
- types
  - index - create an index on only value of columns
  - unique index - create an index on only unique value of columns
```
create index index_name on tb (using method)  method - column name
(
column_name [ASC | DESC} [NULLS {FISRT | LAST}],

-- name for index idx_table_name_column_name
-- name for unique index idx_u_table_name_column_name

create index idx_employees_email on employees (email)
create index idx_employees_email on employees (email,phone)

create index idx_u_employees_email on employees (employee_id)

-- list indexss, table size (host)
select * from pg_indexes
select * from pg_stat_all_indexes

selct pg_indexes_size('aa')      -- table size -- the size is doblue when create the index
selct pg_size_pretty(pg_indexes_size('aa'))   return in kb

-- drop
drop index [concurrently]
[if exists] index_name
[cascade | Restrict ];

-  if any object depenthen restrit the drop else cascade
- block the table to remove the index fully 

```

### SQL statement execution process
- sql is declartive language
- Four stages
  - parse   --- verify syntax & user privileges, disassemble info like * to column name and clauses
  - rewriter  -- apply any syntatic rule  (soft pool / hard pool)
  - optimizer  -- find the very fastest path to the data (lowest cost) -
    - node - for every operation and  ever access method
    - types (sequential scan, index scan/index only/ bitmap index scan, nextedd loop / hash join / merge join , gather and merge parallel join)
    - ` select * from pg_am `
     - sequential scan - default - read dataset from  begining ` explan select * from tb `
     - index node - use index to access the dataset
       - index scan
         - index -> seeking the tuple -> then read again tha data
         -  ` select * from vv where id = 10 `
       - index only scan
         - request index column only -> directly get data from index file
         -  ` select id from vv where id = 10 `
       - BItmap index scan
          - build a memory bitmap of where tuple that satisfy the statement clause
       - join node
         - when use join for table
         - type
           - Hash join (inner table then outer table) ` select * from vv where id in ('') `
           - merge join (two sorted child then need to scan the relation once  )
           - nested loop ( each row iterate the inner table for matching condition)

  - executor  -- run as relational algebra (pi column(&column>100(table))) --pi select & where
- index type
  - btree index
    - default index for both normal and unique index
    - self balanceing tree so log time for seelct insert delete and sequence access
    - normaly used for building primary key indexes
    - uses when < > <= = <> between in is null is not null
  - Hash index
    - for = operator
    - larger then btree
    - ` crete index index_nmae on table using hash (column_name) `
  - BRIN index
    - block range indexes
    - small index
    - data block min to max
    - less cose then btree
    - used in large table and linear sort
  - GIN index
    - Generalized inverted indexes
    - point to multiple tuple
    - uses when the multiple value stored in single column
 ### explain
 - explain give the node type and cost and rows and width
 - execution node - scan type
 - cost how much the node execute
   - startup cost - initial cost
   - final cost
 - rows - how tuple is expected to provide for final dataset
 - width how many bit every tuples will occupy
``` 
explain select * from vv    -- query plan 
explain [ FORMAT JSON ] select * from vv   data output of query plan also in xml, txt yaml
explain analysis select * from vv    -- actual time , planning time , execution time


cost calculation
show seq_page_cost

show cpu_tuple_cost

show cpu_operation_cost

show seq_page_cost
pg_relation_size * seq_page + total_number_of_table_records * cpu_tuple_cost + total_number_of_table_records * cpu_operator_cost


-- index increase the memory size too

vacuum analyzt table name -- to clear the cache memory
```
### partial index
 - create index with where condition
### rebuilding an index
- reindex [ { verbose } ] { index |table | schema | database | system } [concurrently ] name

  



    
