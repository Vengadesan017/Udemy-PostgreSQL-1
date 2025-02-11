# Aggregate Funtion
### count
- select count(*) from aa
- select count(1) from aa  -- both are same
- select count(a_no) from aa
- select count(distinct(a_no)) from aa

### sum
- select sum(a_id) from aa 
- select sum(distinct(a_id)) from aa
### MIN / MAX
- select min(a_id) from aa
- select max(a_id) from aa
### greatest / least
- select greatest(21,23,123,12)
- select *,greatest(a_name,a_no) from aa
- select *,least(a_name,a_no) from aa
### avg
- select avg(a_id) from aa
### use maths to combiine the column
- select 2+2
- select a_name, (a_id - a_id) as name from aa  - only for numaric column



