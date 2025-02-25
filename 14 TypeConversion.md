# Type Conversion
- converting column from one data type to another data type
- types
  - Implicit - conversion done automatically ( use '1' in where clause for filter int column)
  - Explicit - user use conversion function like cast or ::
    - using cast(expression as target_data type)
    - expression - constant, table column , expression
    - target_data_type - boolean, character , numeric, array, JSON, UUID, hstore , tempdata (date time interval)

    ```
    where a_id = integer '1'

    -- using cast(expression as target_data type)
      select cast('2022-11-31' as date)
    -- use ::
      select '2022-11-31'::DATE
    ```

# Conversion Funtion
- to_char()
  - used to convert the timestam, interval, int,double, into a string
  - select TO_CHAR(123,'9,99')  -- int 123 is converted to str as 9,99 format lile 1,23 
  - select TO_CHAR('2021-02-31','DD-MM-YYYY')  -- 31-02-2021 set valure from toble
- to_number()
  - select TO_NUMBER('123.3','999.')  -- 123  use predefined patternce
- to_date()
  - select TO_DATE('022124','MMDDYY')  -- 2024-02-21 use predefined patternce
- to_timestamp()
  - select TO_TIMESTAMP('2020-02-28 10:30:25','YYYY-MM-DD HH:MI:SS')   -- 2020-02-28 10:30:25+05:30
  - select TO_TIMESTAMP('2021-02-31','YY')   -- 2021-01-01 00:00:00+05:30
 


# user define data type





