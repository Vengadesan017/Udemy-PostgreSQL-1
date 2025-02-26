### late year data
```
select * from order
where
extract ('y' from order_date) >= extract('y' from (selct max(oder_date) from orders))
```
### from t1 or t2 
-- use union 

### cube root
select sqrt(4) as "Square Root"
select cbrt(4) as "Cube Root"
