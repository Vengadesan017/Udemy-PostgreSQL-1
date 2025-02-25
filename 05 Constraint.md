# Constraint 
- control the data into db by the rules that prevent invalid data enter
- increase accuracy and reliability
- for both table level and colum table
- column level types
  - NOT NULL   - NO null or no data but allow empty str ''
  - UNIQUE    for single column or combination of 2 column (alter table tb add constaint con_name unique(c1 , c2))
  - DEFAULT  - in table creation add default 'value'
  - PRIMARY KEY - table had 1 or multiple pk(composite pk), no null
    - composite key - allow to conbine 2 coumn to make the pk
      ```
              create table grade (
      course_id int,
      student_id int,
      garde int
      primary key (student_id)

      
        create table grade (
      course_id int,
      student_id int,
      garde int
      primary key (course_id,student_id

      alter table grade
      add constraint grade_pkey__pk primary key (c1,c2);
      
        -- drop
      alter table grade
      drop constraint grade_pkey;
      
      ```
  - FOREIGN KEY
     ```
     create table...
      supplier_id int
       foreign key (supplier_id) reference tabil_name(t_id)

     alter table tb_name
     add constraint c1_fkey foreign key (t2_cn) referance t1(t1_c1)

     alter table tb_name
     drop constraint c1_fkey


     ```
  - CHECK - check the data before insert or update
    ```
    -- in creating table
    column_name datatype check(column_name>'100')

    -- existing table
    alter table table name
    add constraint con_name
    check(c1 > 10 and c2 <10 and c3 == 10)

    -- rename
    alter table table_name
    rename constraint con_name to new_con_name

    alter table table_name
    drop constraint new_con_name
    

    
    ```
