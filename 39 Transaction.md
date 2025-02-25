# Transaction
- bundle multiple step into singal , all or nothing operation
- protect db
- command
  - commit
  - rollback  - reverse to original state
  - savepoint - partial transaction
  - grant
  - revoke

```
begin;
select * from vv
commit;  -- create new connection window for commit

-- creare/open transaction
begin

update vv set name = 'vv' wher id = 10
commit ; -- errror aborted

-- to normal state , remove aboring state because rollback kill all the aboring state
rollback

update vv set name = 'vv' where id = 10 -run  but data is not updated in table

commit ; -- run now thw data is updated in table
```
### FIX from crash
- if crash then postgree rollback to normal state
### Save point
```
begin;

  < vaild_statement >
  savepoint savepoint1;
  <error >
  rollback to savepoint1;
commit:
```

# ACID ATOM
  - Atomicity	- single place full operation
  - Consistency	- valid state before and after a transaction
  - Isolation	- multiple transactions can occur concurrently
  - Durability	- Recovery system failure , upto data
