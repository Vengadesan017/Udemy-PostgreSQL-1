### date / time
- YYYY-MM-DD
- HH:MM:SS
- YYYY-MM-DD HH:MM:SS.p
- time -24hr
  - with time zone 
  - without time zone

### system month date setting
- show DateStyle
- set DateStyle = 'ISO,MDY'
### string to date
- TO_DATE(date,format)
  - format YYYY / Y,YYY /YY / IY MONTH / Month / DAY / DD /D / *****
  - select TO_DATE('2000-10-10', 'YYYY-MM-DD')
  - select TO_DATE('20001010', 'YYYYMMDD')

- TO_TIMESTAMP(timestamp,format)
    - select TO_TIMESTAMP('2000-10-10', 'YYYY-MM-DD')

 - TO_CHAR(timestamp, format)   --to string
  - select TO_CHAR('2000-10-10'::DATE, 'Month')
### date construction funtion
- make_date -convert into date
  - select MAKE_DATE(2000,1,10)
- make_time
  - select MAKE_TIME(11,31,33.05)
- make_timestamp
  - select MAKE_TIMEatamp(2000,1,10,11,31,33.05)
  - select MAKE_TIME(11,31,33.05)
- make_timestamptz
  - select MAKE_TIMEatamptz(2000,1,10,11,31,33.05)
  - select MAKE_TIME(11,31,33.05)
- make_interval
  - select MAKE_TIMEatamp(2000,1,10,11,31,33.05) to readly text
  - select MAKE_TIMEatamp(days => 10) - 10 days
### date value extractors
- extract(field fromsource)  field - month, second,
- select extract('Day' from current_timestamp)
### math ops
- select '20001230::date' + 10
### overlaps ops
- check the time in the given time
- return t or f
- select (Date '' ,Date '') overlap (date '' , date '')   -- check r in l side value

### funtions
- select now(),transaction_timestamp() both are same
- select Statement_timestamp()    -exec time
- select timeofday()
- select age('2000-01-21','2000-02-21')  -- differnece of this date
- select current_date , current_date -1
- select current_time
- epoch is used for accuracy

### view and set time zone
- select * from pg_timezone_names
- show time zone
- set time zone 'us/Alaska'
- to change the time zone of table
- ` alter table tb add column time with time zone '
- ` insert into tb (time) values (2000-10-31 11:11:11 US/Pacific)

### date_part - get specific field like year month day hour
- select date_part('year' , timestamp '2000-12-12')    -- 2000
### date_trunc
- truncate the timestamp or interval
- - select date_trunc('year' , timestamp '2000-12-12')    -- 2000-01-01 00:00:00
