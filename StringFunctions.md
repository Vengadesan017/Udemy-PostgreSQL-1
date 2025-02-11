# string Functions

### UPPER - convert to uppercase / LOWER / INITCAP
- select a_name, upper(a_name) as upper_n from aa
- select a_name, lower(a_name) as lower_n from aa
- select a_name, initcap(a_name) as name from aa
- select initcap(concat(a_name,' ' a_code)) as name from aa
### left / right - return first n character
- select a_name, right(a_name,1) as name from aa 
- select a_name, right(a_name,-1) as name from aa  return all char except 1 from left
### REVERSE
- select a_name, REVERSE(a_name) as name from aa
### SPLIT_PART
- split_part(string,delimiter,position)
- select split_part('1,2,3,4',',',4)   -- 4 -- nth value based on dilimiter
- select split_part('0,2,3,4',',',1)   -- 0 -- nth value based on dilimiter
### TRIM / LTRIM(left) / RTRIM(right) / BRIM(both r&b)
- remove spaces by default and also we trip by char or text
- trim([leading | trailing | both ] [characters] from string)
- ltrim(string, [character])
```
select trim(leading from ' haii  ')
select trim(trailing from ' haii  ')
select trim(both from ' haii  ')
select trim(' haii  ')   --both
select rtrim(' haii  ') 
select ltrim(' haii  ') 
select btrim(' haii  ')

select trim(leading '0' from cast(0000123 as text))
select ltrim('yyyyqywert','y')
```
### LPAD / RPAD
- give padding to fill the content
```
  select lpad('sample',10,'#')
  select rpad('sample',10,'#')
``` 
### LENGTH
- select length ('sdfdsvdv)
### position , strpos 
- return position of sub string
- case sensitive
- ` select position('fd'in 'efesdsfddfd')   -7 `
- ` select strpos('efesdsfddfd', 'fd') -7`

### SUBSTRING
- select a string from this based on the length not a index
  ```
   select substring('the main string 123123' from 5 for 6)
  select substring('the main string 123123' for 6)
  ```
### REPEAT , REPLACE
  ```
   select repeat('the main string 123123' , 6)

   select replace('the main string 123123' , 'the' ,'The')

  ```
