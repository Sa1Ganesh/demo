SQL (to primary key)

> set oracle_sid=xe/orcl
to check id > echo %oracle_sid%
database connecting >sqlplus un/pw
>sqlplus system/Sathya
>sqlplus /nolog
>conn system/Sathya
>sqlplus system/sathya as sysdba
>sqlplus system/sathya as sysdba@192.168.-.-:1521/xe
>show user
>select * from global_name;
>select * from v$version;
>quit

> archive log list;
Database log mode              Archive Mode
Automatic archival             Enabled
Archive destination            C:\app\ganesh\product\21c\homes\OraDB21Home2\RDBMS
Oldest online log sequence     14
Next log sequence to archive   16
Current log sequence           16

if no archive mode
then
>shutdown immediate;
>startup mount
>alter database archivelog;
>alter database open;
>select log_mode from v$database;



SID:
SID stands for System Identifier which is a unique name for your database. By default its either ORCL or XE. You can check your SID by querying V$DATABASE view or V$THREAD.

select expression from table;
select * from table;
select column list from table;
select id+1 from table1;

Concatenation operator
select column1||column2 from table;
select name||' '||place from table1;
select 'the id is'||' '|| id from table1;
select 'name is'||' '|| name from table1;

DISTINCT
to eliminate duplicate rows from result set
select distinct column1,column2 from table;

Single Row Functions
1.Character functions
i.Case-manipulation functions
1.lower()
ex:select lower(name) from table1;   
select lower('sai ram') from dual;  
 (dual is a dummy table provided by oracle)
  select user ,sysdate from dual;
2.upper()
ex:select upper(name) from table1;
select upper('sai ram') from dual;
3.initcap()
ex:select initcap(name) from table1;
select initcap('sai ram') from dual;

ii.Character manipulation functions
1.concat() only two operators
syntax:concat(string1,string2) = string1||string2(|| as many u wnt)
select concat(name,place) from table1;
select concat(concat(name,' '),place) from table1;
select name||' '||place from table1;
2.substr()
Substr ( source_string, start_pos, Substr_length )
SQL SUBSTR () function will return a sub string of a specified length from the source string beginning at a given position.
Source string can be of any data type CHAR, VARCHAR2, NCHAR, NVARCHAR2, CLOB, or NCLOB whereas the other two parameters that are start_pos and Substr_length must be of number data type.
If you have supplied a numeric string instead of character as source string, the oracle engine casts them as a character when they occur as parameter to SQL Substr function. And if you have supplied Arithmetic expression or a DATE then The Oracle engine first solves or evaluates the Arithmetic expression or the DATE & then it casts them as a character.
ex:
select substr('ilovesathyaforever',0,11) from dual;
select substr('ilovesathyaforever',1,11) from dual;
select substr('ilovesathyaforever',6,13) from dual;
select substr('ilovesathyaforever',-13,13) from dual;
select substr('ilovesathyaforever',0) from dual;
select substr(59999-7,0,5) from dual;
select substr(sysdate,0,8) from dual;

3.length()
4.instr()
5.rpad()|lpad()
6.trim()
7.replace()

2.number functions
3.general functions
4.conversion functions
5.date functions

Tables in RDBMS are a data-structure which helps in organizing data in Rows and Columns.

CREATE TABLE table_name  
 (
  Column_name1  Data-Type(size)  Constraint,
  Column_name2  Data-Type(size)  Constraint,
   …
  Column_name1000  Data-Type(size)  Constraint
 );

Table names and column names:

Must begin with a letter
Must be 1–30 characters long
Must contain only A–Z, a–z, 0–9, _, $, and #
Must not duplicate the name of another object owned by the same user
Must not be an Oracle server–reserved word

To create a table in your schema you will require CREATE TABLE system privilege.
There are 3 different ways for creating a table.
1.using CLI command line interface in command prompt
2.using GUI graphic user interface in oracle database(sql developer)
3.using EM enterprise manager

Primary Key Constraint

Primary key constraint is an Input/output Data constraint which serves the purpose of uniquely identifying the rows in a table.
Primary key constraint is the combination of NOT NULL and UNIQUE constraints.

There are two types of Primary keys:

1.Simple Primary Key 
A primary key constraint that is defined using only one column of a table in the database is called Simple Primary Key Constraint.

2.Composite Primary Key.
A primary key constraint that is defined using more than one column of a table in the database is called Composite Primary key constraint.

a composite primary key can be defined using up to 32 columns of a table in oracle database.

Oracle automatically creates a unique index so that the requirement of the uniqueness of the PRIMARY KEY constraint is fulfilled.

Using CREATE TABLE statement and
Using ALTER TABLE statement

you have two different levels at which you have to define a primary key.

Primary Key at Column Level

CREATE TABLE product_master
 (
  Product_id  NUMBER(3)  PRIMARY KEY,
  Product_name  VARCHAR2(30),
  Product_price  NUMBER(5)
 );

You can use keyword “CONSTRAINT” to give your constraint a meaningful name.
How to name a primary key constraint

CREATE TABLE product_master
 (
  Product_id   NUMBER(3)   CONSTRAINT   promstr_col1_pid_pk   PRIMARY KEY,
  Product_name   VARCHAR2(30),
  Product_price   NUMBER(5)
 );

Primary Key Constraint at Table Level

you first define all the columns of a table and then you define all your constraints in the Create Table Statement. 

CREATE TABLE product_master
 (
  Product_id  NUMBER(3), 
  Product_name  VARCHAR2(30),
  Product_price  NUMBER(5),
  CONSTRAINT promstr_col1_pid_pk PRIMARY KEY (product_id)
  );

2.You can use ALTER TABLE statement to add the constraint in an already created table or to change the definition of already defined constraint in the table.

ALTER  TABLE  customer  ADD  CONSTRAINT  cust_cid_pk  PRIMARY  KEY  (cust_id);

Composite Primary Key Constraint

the primary key defined using more than one column is called a composite primary key. 

CREATE TABLE customer
 (
  cust_id NUMBER(3),
  cust_name VARCHAR2(3),
  phone_no NUMBER(10),
  CONSTRAINT cust_cid_pk PRIMARY KEY ( cust_id, phone_no)
 );

 Modifying a constraint becomes easier if your constraint has a unique and meaningful name. If you do not provide a name to your constraint then oracle server gives a default name to it which is quite generic and it becomes difficult to find a specific constraint using that name.
 to enable or disable it using the name of the constraint.

ALTER TABLE customer DISABLE CONSTRAINT cust_cid_pk;
ALTER TABLE customer ENABLE CONSTRAINT cust_cid_pk;

DESC statement shows nothing about constraints on a table. But Oracle Database provides us several DATA DICTIONARIES for checking or describing all the constraints which we have defined on our table.

USER_CONSTRAINTS gives a brief about the constraint on a table. 

USER_CONS_COLUMNS is a Data Dictionary which holds detailed information about columns of a table.

USER_CONSTRAINTS

SELECT
  CONSTRAINT_NAME,
  CONSTRAINT_TYPE,
  TABLE_NAME,
  STATUS,
  INDEX_NAME
 FROM user_CONSTRAINTS WHERE table_name = ‘CUSTOMER’;

USER_CONS_COLUMNS

SELECT 
  CONSTRAINT_NAME,
  TABLE_NAME,
  COLUMN_NAME,
  POSITION
 FROM user_cons_columns WHERE table_name = ‘CUSTOMER’;

Restrictions on a primary key constraint
You cannot delete a Primary key if it is referenced by a foreign key in some other table.
A table can have only One Primary key no matter whether it’s Simple Primary Key Or Composite Primary Key.
Columns which are participating in Primary Key cannot have NULL values. This means you cannot leave them unattended or you cannot put NULL value into them.
As primary key is all about row or record’s uniqueness thus it will not allow duplicate values.
When a Primary Key constraint has been defined on multiple columns then its values can be duplicated provided the duplication is happening within one single column. However the combination of values of all the columns defining each primary key constraint should be unique.
Data-types such as LOB, LONG, LONG RAW, VARRAY, NESTED TABLE, BFILE, REF, TIMESTAMP WITH TIME ZONE, or user-defined type are not allowed with the columns which are part of Primary key. Any attempt of creating a primary key with the column of these data-types will raise SQL Error: ORA-02269.
The size of the primary key cannot exceed approximately one database block.
As I have already mentioned above that a composite primary key can have maximum of 32 columns.
The Primary Key and Unique Key should never be designated as the same column or combination of columns.
You cannot specify a primary key when creating a sub view in an inheritance hierarchy. The primary key can be specified only for the top-level (root) view.
Unique cluster Index gets created automatically at the time of creating Primary key.
Although it is not necessary for you to define a primary key yet it is always recommended to do so.

SQL(foreign key)

Foreign key
is an Input/output data constraint which is also known as referential integrity constraint. Foreign key represents a link or say a relationship between columns of tables.

Constraint which involves only one column in foreign key in child table and one column in reference key in parent table is called Simple Foreign Key.

While the constraint which involves more than one column in foreign key in the child table as well as more than one column in reference key in the parent table is called Composite Foreign Key.

Features of Foreign Key

1.You cannot define a foreign key constraint in a CREATE TABLE statement that contains an AS sub query clause. Instead, you must create the table without the constraint and then add it later with an ALTER TABLE statement.
2.None of the columns in the foreign key can be of LOB, LONG, LONG RAW, VARRAY, NESTED TABLE, BFILE, REF, TIMESTAMP WITH TIME ZONE, or user-defined type. However, the primary key can contain a column of TIMESTAMP WITH LOCAL TIME ZONE.
3.4.A composite foreign key cannot have more than 32 columns.
Referenced key in parent table must either be a Primary Key or a Unique Key.
5.Records in parent table cannot be updated if child record exists.
6.Foreign Key Constraint can be specified only on child table and not on parent table.

Foreign key involves two different tables. First one is PARENT TABLE or referenced Table and the second one is CHILD TABLE or foreign table.

The column of parent table which will get referenced by foreign key must be either Primary Key or Unique Key.
Column(s) in child table can contain NULL or Duplicate values while the vice versa is not true.
Column(s) of parent table & column(s) of child table which are participating in foreign key should be of the same data-type and size (column width).

1.Defining Foreign Key Using Create table at Column Level
This way of defining constraint is called column level because we define the constraint with column definition while creating table.

Syntax  

Column_name   Datatype(size)   REFERENCES   parent_table_name (parent_column_name)

2.How to define foreign key using CREATE TABLE at table Level

CONSTRAINT   constraint_name   FOREIGN KEY(child_table_column)   REFERENCES   Parent_table_name(parent_table_column)

ALTER TABLE  child_table_name  ADD  FOREIGN KEY  (child_column)  REFERENCES  parent_table_name(parent_column)

If you try to delete a parent table which has a primary or a unique key referenced by child table then oracle will give you a SQL Error: ORA-02449. However if you want then you can still drop the child table without any error.

The foreign key is defined in the child table and the parent table contains the reference column. However because of this link we cannot update or delete the rows of the parent table.

When you define a simple foreign key, the Oracle engine is by default set to ON DELETE NO ACTION clause. This means that you are allowed to update the rows in the parent table however you cannot delete rows from the parent table. This default behavior is called Restrict rule. This rule doesn’t allow users to delete or update reference data in the parent table.

You can easily override this restrict rule and change the default behavior of the foreign key either to SET NULL or to DELETE CASCADE.

1.On Delete Set Null clause
 sets all the records of the column which is defined as a foreign key in the child table to Null if the corresponding record in the parent table is deleted.

ex:
create table table1(id_1 number,name_1 varchar2(30));
create table table2(id_2 number,name_2 varchar2(30));
insert into table1 values(1,'sai');
insert into table1 values(2,'sita');
insert into table2 values(1,'ram');
insert into table2 values(2,'rama');
select * from table1;
select * from table2;
drop table table1;
drop table table2;
alter table table1 add constraint pk primary key(id_1);
alter table table2 add constraint fk foreign key(id_2)  references table1(id_1) on delete set null;
delete from table1 where id_1=1;
delete from table2 where id_2=2;

2.On Delete Cascade clause of foreign key

deletes the dependent column entry in child table when there is any attempt of deleting the corresponding value in Parent table.

ex:
alter table table2 add constraint fk foreign key(id_2)  references table1(id_1) on delete cascade;

SQL

Syntax of changing TABLE name :
ALTER TABLE  old_name_of_table  RENAME TO  new_name_of_table;

How To add in an Existing Table
ALTER TABLE  existing_table_name  ADD  new_column_name  data_type (size);

How To Drop A Column In A Table
ALTER TABLE  existing_table_name  DROP COLUMN  column_name;

How To Rename & Modify A Column
ALTER TABLE  table_name  RENAME COLUMN  old_name_of_column  TO  new_name_of_column;
ALTER TABLE  table_name  MODIFY  colun_name  data_type (size);

ex:
create table table1(id_1 number,name_1 varchar2(30));
alter table table1 rename to table_1;
alter table table1 add place varchar2(30);
alter table table1 modify place varchar2(50);
alter table table1 rename column name_1 to name;
alter table table1 drop column place ;

INSERT INTO   table_name   (col1, col2…)   VALUES   (val1, val2…);
INSERT INTO   table_name   VALUES   (val1, val2…);
Insert-Into statement inserts only one row at a time.
Character and date values must be enclosed in single quotation marks.
When inserting records into a table using the SQL INSERT statement, you must provide a value for every NOT NULL column.
You can omit a column from the SQL INSERT statement if that column allows NULL values.
If you are specifying column names in the query then the order in which the values are being specified must be the same. On the contrary, if you are not specifying the column names even then your order of values should match with the order of the columns in your table.

To copy a table
create table table1(id_1 number,name_1 varchar2(30));
create table table2 as select * from table1;

Copy /Insert data into a table from another table using INSERT INTO SELECT
insert into table2 select * from table1;
insert into table2(column_t2) select column_t1 from table1;

DELETE FROM  table_name  WHERE  condition;

Truncate command deletes all the rows from a table.
TRUNCATE TABLE table_name;

Truncate is a DDL command.
You cannot roll back a TRUNCATE statement.
You cannot use WHERE clause with TRUNCATE statement. This means that unlike SQL DELETE there are no options for conditional delete.
Unlike SQL delete when you perform Truncate no trigger gets executed.
Truncate doesn’t use much UNDO space as SQL Delete.
Truncate is significantly faster than the SQL Delete. This is because when you use DELETE, all the data first gets copied onto the Undo Tablespace after which the delete operation is performed. That’s why when you type ROLLBACK after deleting a table; you can get back the data. All this process takes time. But when you type TRUNCATE, it removes data directly without copying it onto the Rollback Tablespace. That’s why TRUNCATE is faster. Once you truncate you can’t get back the data.


Joins

Joins are SQL operations which help us in retrieving data from two or more tables that share a common field.

 Types of Joins 
Inner Join
Outer Join
Cross Join
Self-Join

Outer Join can further be divided into 3 more categories:

Right Outer Join
Left Outer Join
Full Outer Join

Outer Join: Outer joins are the SQL operations which definitely return all the rows from Source table no matter whether there is a matching join condition hit or not. At the same time it returns only those rows of the target table that fulfills the matching join condition otherwise it just shows ‘null’ in the rows.


SELECT 
    columns 
  FROM 
  source_table JOIN target_table 
  ON(expression)/using(column_name)
  WHERE condition
  ORDER BY column_names;

1.A NATURAL JOIN
is a JOIN operation that creates an implicit join clause for you based on the common columns of the two tables that are being joined. Common columns are columns that have the same name in both the tables.
A NATURAL JOIN can be an INNER join, a LEFT OUTER join, or a RIGHT OUTER join. However the INNER join is the default one.

Source table: The table which comes after FROM clause in the select statement. 

Target table: All the tables that come after JOIN clause in the query.

When the join query is executed then the oracle starts matching the data from the source table to the target table. If there is a hit for a matching source table data in the target table then the value is returned. 

The best part of using Natural join is that you do not need to specify the join column because column with the same name in source and target tables are automatically associated with each other. 

The queries in which all the common columns of source and target table get associated automatically are known as Pure Natural Join.

using ON clause then it becomes mandatory to specify the columns over which we want to perform the join.

SELECT   first_name, department_name   FROM   employees    NATURAL JOIN   departments;

SELECT   first_name,department_name   FROM   employees  JOIN    departments   ON   (employees.manager_id = departments.manager_id   AND   employees.department_id = departments. department_id);

SELECT   first_name,department_name   FROM   employees   JOIN   departments   USING(manager_id);

create table table1(id_1 number,name_1 varchar2(30));
create table table2(id_1 number,name_2 varchar2(30));
select name_1,name_2 from table1  natural join table2;
select name_1,name_2 from table1 t1  join table2 t2 on t1.id_1=t2.id_1;
select name_1,name_2 from table1 t1  join table2 t2 using(id_1);

Inner join

SELECT column names FROM 

  table1 INNER JOIN /  JOIN table 2

  ON(expression) or USING(column_name) 

  WHERE(expression) ORDER BY column names;

select name_1,name_2 from table1  natural join table2;
select name_1,name_2 from table1 t1 inner join table2 t2 on t1.id_1=t2.id_1;
select name_1,name_2 from table1 t1  join table2 t2 on t1.id_1=t2.id_1;
select name_1,name_2 from table1 t1  join table2 t2 using(id_1);

2.Right Outer Join 
is a form of Outer Join that returns each and every record from the source table and returns only those values from the target table that fulfil the Join condition.

*Note: The Source table is the one situated on the right side of the Right Outer Join Clause whereas The Target table is the one on the left side of this clause.

SELECT 
    columns 
  FROM 
  target_table RIGHT OUTER JOIN source_table 
  ON(source_table.column = target_table.column)
  WHERE condition
  ORDER BY column_names;

We use ON Join Condition in SQL Joins 
When columns which are participating in ON join condition have Different name and Same Data type or
When columns which are participating in ON join condition have SAME NAME and SAME Data-type.

Using clause can be used in the place of ON clause when
We are performing Equi Join.
We want to put join on that column of source and target table which are common i.e. have same name and data type.

select name_1,name_2 from table1 t1 right join table2 t2 using(id_1);
select name_1,name_2 from table1  t1 right join table2 t2 on t1.id_1=t2.id_1;

3.Left outer join

SELECT 
    columns 
  FROM 
  source_table left OUTER JOIN target_table 
  ON(source_table.column = target_table.column)
  WHERE condition
  ORDER BY column_names;

select name_1,name_2 from table1 t1 left join table2 t2 using(id_1);
select name_1,name_2 from table1 left join table2 on t1.id_1=t2.id_1;

4.Full outer join 
is kind of a combination of both right outer join and left outer join because it returns all the rows from the left as well as the right side table.

SELECT column names FROM 

  table1 FULL OUTER JOIN / FULL JOIN table 2

  ON(expression) or USING(column_name) 

  WHERE(expression) ORDER BY column names;

select name_1,name_2 from table1 t1 full outer join table2 t2 using(id_1);
select name_1,name_2 from table1  full join table2 using(id_1);

5.Cross join
 Cross Join produces Cartesian product of the tables which are participating in the Join queries that’s why it’s also known by the name of Cartesian Join.

Generally we use CROSS JOIN when there is no relationship between participating tables.

Syntax

SELECT  column names  FROM  table1  CROSS JOIN  table 2  WHERE (expression)  ORDER BY  column names;

or

SELECT  column names  FROM  table1,  table 2  WHERE (expression)  ORDER BY  column names

you can write either Cross Join or just put a comma in between the names of both tables. 

Also with cross join we do not have any ON or USING join condition. But you may, however, specify a WHERE and ORDER BY clause.
 if you are using ORDER BY clause then make sure it must be the last statement of your SQL query.

select name_1,name_2 from table1 cross join table2;
select name_1,name_2 from table1,table2;
select name_1,name_2 from table1 cross join table2 order by name_1;

NEW USER

Create a new user using Create user statement.
Using the Oracle “create user” statement you can create and configure new database user and by using this database user you can log on to your database.

Syntax of The Oracle Create User statement

CREATE USER username IDENTIFIED BY password/externally/globally 
DEFAULT TABLESPACE tablespace_name TEMPERORY TABLESPACE tablespace_name 
QUOTA size/unlimited ON tablespace_name 
PROFILE profile_name 
PASSWORD expire 
ACCOUNT lock/unlock;

we have IDENTIFIED BY clause which helps the Oracle engine in authenticating the user. There are three ways of authenticating a user

1.Password: Password clause lets you create a local user and indicates that the user must specify a password to log on to the database. Remember here: passwords are case sensitive. Passwords can consist of only single-byte characters from your database character set regardless of whether the character set also has multi-byte characters.
2.Externally: This clause helps you in creating an external user. These external users and their authentication are managed by external services such as the operating system. The user who is authenticated by the operating system can easily access the database without being requested for a password. External users are classic regular database users (non-database administrators) who are assigned standard database roles (such as CONNECT and RESOURCE), but no SYSDBA (database administrator) or SYSOPER (database operator) privilege.
3.Globally: Through Global users, you can keep the login or authentication information at a Central Oracle Security Server rather than having passwords in every database to be accessed. Moreover, on the assignment of a global user, the authorization information can be assigned globally as well. This means that you can assign global roles to global users in the central Oracle Security Server, instead of assigning the roles in each database.

Default Tablespace
In the default tablespace, you have to specify the name of tablespace where this user will create its object. By objects I mean tables, indexes, synonyms etc. This is an optional clause, if you omit this clause then all the objects created by this user gets stored in the database default tablespace which is “users” tablespace in most cases.

Temporary Tablespace
All the temporary segments created by this user get stored here. Temporary segment such as temporary tables; Similar to default tablespace you have to specify the name of the temporary tablespace here. But make sure the tablespace you are going to specify here must be temporary tablespace. This is an optional clause so if you omit this clause then all the temporary objects created by this user get stored in the database default temporary tablespace which is “temp” tablespace.
Always remember the name of tablespace you will specify here must be a temporary tablespace and should have a standard block size. Also, it should not be an undo tablespace or a tablespace with automatic segment-space management

Quota Clause
Quota clause can help you in specifying the maximum amount of space that the user can allocate in the tablespace. In single create user statement you can have multiple quota clause. As you can see here our quota keyword is followed by size. You can either specify a limited size such as 50M or 100M, M stands for Megabyte here or just write UNLIMITED for allocating space without any limits. After the size, you have to specify the name of tablespace using ON clause.

Profile Clause 
Using profile clause you can specify the profile you want to assign to the user. By Assigning profile you can limit the amount of database resources the user can use.
It’s also an optional clause hence if you omit this clause database will assign a default profile to your user.

Password Expire Clause
This is the kind of settings which force your user to change the password on the first login attempt or say before the user can log in to the database. This is also an optional clause.

Account Clause
Using Account clause you can set the status of your user account. If you set the status on the lock then Oracle will create your user account but disable its access. This means that you will have your user account but you cannot use it. If you set the status on UNLOCK then you are all free to use it. Unlock means status will be open. By default its set on UNLOCK option. 
It’s again optional if you omit this clause. In case you do omit it then the Oracle will set the status of your account to default state on UNLOCK.

ex:
create user sai identified by love
default tablespace users temporary tablespace temp
quota 20M on users
account unlock;

select username,account_status,default_tablespace,temporary_tablespace,profile,authentication_type from dba_users where username='SAI';

should grant create session system privileges to the user

grant connect session to sai;

To Drop a user in Oracle Database.

To Drop a user you will either need a drop user system privilege or SysDBA privilege (Log on to the database using Sys user with SysDba privileges)

How to drop a user when it is not connected to the database

Suppose you have a user by the name of John and you want to drop it. To drop this user you can use DROP USER DDL statement.

DROP   USER   John;

he above statement will drop the user John. This is the situation when user does not own any objects. By objects I mean tables, indexes, view etc. In case your user owns these objects then database compiler will raise an error and will not allow you to drop the user. In this situation you will have to drop the user with cascade option.

For example, Say user john has tables, indexes and several other objects and you want to drop it. In this case you will have to use cascade drop user statement.

DROP   USER   john   CASCADE;

There are few things which you should know

1.If the user’s schema contains tables, then Oracle Database drops the tables and automatically drops any referential integrity constraints on tables in other schemas that refer to primary and unique keys on these tables.
2.If this clause results in tables being dropped, then the database also drops all domain indexes created on columns of those tables and invokes appropriate drop routines.
3.Oracle Database invalidates, but does not drop, the following objects in other schemas:

-Views or synonyms for objects in the dropped user’s schema
-Stored procedures, functions, or packages that query objects in the dropped user’s schema
-Oracle Database does not drop materialized views in other schemas that are based on tables in the dropped user’s schema. However, because the base tables no longer exist, the materialized views in the other schemas can no longer be refreshed.
-Oracle Database drops all triggers in the user’s schema.
-Oracle Database does not drop roles created by the user.
 
DROP USER is a DDL statement. DDL statement does not support ROLLBACK command which means once you have dropped the user you cannot get it back using ROLLBACK command.

How to drop a user when it is connected to the database

alter system enable restricted session;
select sid,serial# from v$session where username='SAI';
alter system kill session '25,638';
drop user cascade;
alter system disable restricted session;


Users that are also known as schemas are required when you work with your database. They let you connect with the database and access the stored data.

There are two types of Schemas in oracle database.

1.Manually Created Schema
These schemas that are created by you or by the database administrator for the end users such as database developers who need to work with your database.

Sample Schema or Users
These schemas come preinstalled with your database for practice purposes. By default these schemas are locked. In order to use them you have to first unlock them.
we will learn how to unlock sample users in oracle database using ALTER USER DDL statement.

How to Unlock Sample User/ Schema in oracle database.

Oracle database comes with several sample schemas pre-installed for practice purposes. The best thing about these schemas are that they already have dummy data installed and have been created while keeping in mind all the database creation guidelines. This means that you do not have to do anything. Just unlock them and use them.
 
Windows Command Prompt.
1.Set the Oracle SID.
Set ORACLE_SID=xe
2. Connect to your database.
you have to connect to your database using the user with SYSDBA privileges or any user which either has SYSDBA privileges or system privileges or granted with ALTER USER privilege. 
SQLPLUS sys/oracle AS sysdba
3. Unlock user with ALTER USER DDL
ALTER USER username IDENTIFIED BY password ACCOUNT unlock;

Unlock User or Schema In Oracle Database Using SQL Developer.

Step 1: Logon to the database.
To unlock a user in oracle database you either need to have “Alter user system privilege” or log on to your database using sys user with “SYSDBA” privileges.

Step 2: Find The Oracle Database User.
First double Click on The SYS connection and expand it.
Second find the “Other User” folder and expand it also .Under this folder you will find all the users created in your database find the one which you want to unlock.

3. Unlock The User.
Once you locate your user “Right Click” and select “Edit User” doing so will open up an edit user wizard which will let you fully customize your database user.

Step 4: Edit User Wizard
On the front tab of this wizard you can see 3 or 4 checkboxes depending on the version of SQL Developer you are using. Among them there is a checkbox with the name “Account is locked.” You will find this checkbox checked which indicates that the account is locked and not accessible. In order to unlock the user you have to un-check this checkbox.

Also if you have forgotten the password or you don’t know the default password or you are unlocking sample user accounts for the first time then I highly recommend you to set the password here. Otherwise you won’t be able to login.

Once you have finished with all the steps here on the edit user wizard then click apply and come out.


Create an External user using the Create User statement.

External users and their authentication are managed by external services such as operating system or a network service. The user who is authenticated by the operating system can easily access the database without being requested for a password. This means that whenever an external authentication option is chosen for the user – an external service performs the password administration and user authentication whereas the Oracle Database maintains the User account.

External users are classic regular database users (non-database administrators) who are assigned standard database roles (such as CONNECT and RESOURCE), but no SYSDBA (database administrator) or SYSOPER (database operator) privilege.


The name of an external user is a string which is a combination of three factors.
These three factors are:


Hostname
This is your computer’s name. You can very easily find the hostname of your machine, by simply writing hostname in your command prompt and hitting ‘enter’. This will show you the hostname of your machine. Note here – A Hostname is only required when you are creating an external user on windows machine but if you are using a UNIX or Linux machine then there is no need of a hostname.

User account name of your operating system
Second factor is the User account name of your operating system. This is the name of user account for which you want to create the external user. Mind here that the authentication information of this account such as password will serve as the authentication information of your external user.
To find the user name of your OS simply write 

echo %username%

OS_AUTHENT_PREFIX, a special Oracle parameter.
The third factor here is OS_AUTHENT_PREFIX which is a special oracle parameter. This parameter is responsible for telling the oracle that the user which we are going to create is an external user and not a local one or for that matter any other type of user. The name of all the external users must start with the value of this parameter.
To get the value of this parameter first you need to log on to your database using sys user with sysdba privileges. And then write 

SQL>Show Parameter os;

In the fourth line of the output returned by above command, you can see the value of OS_AUTHENT_PREFIX parameter which is by default set as OPS$

Note:
The text of the OS_AUTHENT_PREFIX initialization parameter is case-sensitive on some operating systems. Refer to your operating system specific Oracle documentation for more information about this initialization parameter.


Now let’s create the name of our external user. For that let’s suppose the value of our hostname is “loclalhost” and the name of user account for which we want to create the external user is “Manish” and the value of OS_AUTHENT_PREFIX parameter is set on default “OPS$”

While creating the name of the external user you have to keep certain things in mind such as:

Name of the external user must always start with the value of OS_AUTHENT_PREFIX parameter followed by the hostname and then a backward slash (\) after which comes your user account name.
So the name of external user in Windows environment will be 

OPS$loclalhost\Manish


To create an external user we will  use the Create User statement. 
SQL>CERATE USER “OPS$LOCALHOST\MANISH” IDENTIFIED externally;


Always remember to enclose the name string in double quotes as we have $ sign in the string and this name string should be in all caps.

 Now we have to grant this user a special as well as mandatory system privilege which is “Create Session” so that we can log on to database using this user. 

SQL> GRANT create session TO “OPS$LOCALHOST\MANISH”;

Now as it’s an external user this means that all its authentications are handled by operating system. Thus to log on to our database using this user we have to simply write

C:\> sqlplus /

As our user is an external user thus there is no need of writing username or password, just write forward slash (/) and hit enter.
Check out the quick video on how to create an external user.
   
User Privileges and Roles

  A user privilege is the right to run a particular type of SQL statement, or the right to access an object that belongs to another user, run a PL/SQL package, and so on.

There are two types of user privileges in Oracle Database.

1.System Privilege and
2.Object Privilege

1.System privileges
 are some powerful special rights given to a user to perform standard administrator tasks in the database. These tasks can be any action on any schema objects for example create and drop a user or tablespace, flashback or lock any table and export as well as import the database and many more. 

 Powerful set of rights therefore when in wrong hands they could result in catastrophic events. Hence these should be granted very responsibly and only when absolutely necessary,

2.Object Privilege
Object privileges are the rights given to a user to access and perform some actions on the database objects or the objects owned by some other user in the database.

By objects I mean tables, views, sequences, procedures etc. and by Action I mean Alter, Delete, Execute, Select, Update, References, Insert and Index. 

Object privileges vary from object to object and an owner of the object has all the privileges on it.

To assign and deny the user privilege we have two Data control language (DCL) statements in Oracle database. 

1.Grant and
2.Revoke

Who Can Grant or Revoke System Privileges?

The most powerful administrative user in the database sys can grant and revoke any privilege from any user in the database.
A user with the system privilege GRANT ANY PRIVILEGE and
Any user who was granted a specific system privilege with the ADMIN OPTION
All of these can grant and revoke any privilege. 

In oracle database we have three options which we can use along with the Grant statement and add some flexibility to this privilege management.

These 3 options are:

1.With Admin Option
With Admin Option enables the grantee to –

Grant the privilege or role to another user or role, unless the role is a GLOBAL role
Revoke the privilege or role from another user or role
Alter the privilege or role to change the authorization needed to access it
Drop the privilege or role
If you grant a system privilege or role to a user without specifying WITH ADMIN OPTION, and then subsequently grant the privilege or role to the user WITH ADMIN OPTION, then the user has the ADMIN OPTION on the privilege or role.

To revoke the ADMIN OPTION on a system privilege or role from a user, you must revoke the privilege or role from the user altogether and then grant the privilege or role to the user without the ADMIN OPTION.

2.With Grant Option
WITH GRANT OPTION enables the grantee the power of granting object privileges to various other users. 

Also it is important to remember that WITH GRANT OPTION only assists in granting a privilege to PUBLIC or user and not to a role. 

3.With Hierarchy Option

WITH HIRARCHY OPTION enables the grantee to grant particularly specified object privilege on every sub-objects of an object for example sub-views that are created within a view. This particular clause works only when used in combination with the SELECT object privilege.

Restrictions on Granting System Privileges and Roles

-A privilege or role cannot appear more than once in the list of privileges and roles to be granted.
-You cannot grant a role to itself.
-You cannot grant a role IDENTIFIED GLOBALLY to anything.
-You cannot grant a role IDENTIFIED EXTERNALLY to a global user or global role.
-You cannot grant roles circularly. For example, if you grant the role banker to the role teller, then you cannot subsequently grant teller to banker.

Restriction on Object Privileges

A privilege cannot appear more than once in the list of privileges to be granted.

Security guidelines for user privileges

1.Do not provide database users or roles more privileges than are necessary. (If possible, grant privileges to roles, not users.) In other words, the principle of least privilege is that users be given only those privileges that are actually required to efficiently perform their jobs.

To implement this principle, restrict the following as much as possible:

-The number of SYSTEM and OBJECT privileges granted to database users.
-The number of people who are allowed to make SYS-privileged connections to the database.
-The number of users who are granted the ANY privileges, such as the DROP ANY TABLE privilege. For example, there is generally no need to grant CREATE ANY TABLE privileges to a non-DBA-privileged user.
-number of users who are allowed to perform actions that create, modify, or drop database objects, such as the TRUNCATE TABLE, DELETE TABLE, DROP TABLE statements, and so on.

2.Restrict the CREATE ANY JOB, BECOME USER, EXP_FULL_DATABASE, and IMP_FULL_DATABASE privileges. These are powerful security-related privileges. Only grant these privileges to users who need them.

3.Do not allow non-administrative users access to objects owned by the SYS schema.

4.Lock and expire default (predefined) user accounts.

5.Revoke access to the following:
-The SYS.USER_HISTORY$ table from all users except SYS and DBA accounts
-The RESOURCE role from typical application accounts
-The CONNECT role from typical application accounts
-The DBA role from users who do not need this role

6.Grant privileges only to roles.
Granting privileges to roles and not individual users makes the management and tracking of privileges much easier.

7.Limit the proxy account (for proxy authorization) privileges to CREATE SESSION only.

8.Discourage users from using the NOLOGGING clause in SQL statements.

In some SQL statements, the user has the option of specifying the NOLOGGING clause, which indicates that the database operation is not logged in the online redo log file. Even though the user specifies the clause, a redo record is still written to the online redo log file. However, there is no data associated with this record. Because of this, using NOLOGGING has the potential for entering malicious codes that can be accomplished without an audit trail.

1.System privileges With Admin Option
  
To grant a system privilege, you must either have been granted the system privilege with the ADMIN OPTION or have been granted the GRANT ANY PRIVILEGE system privilege.

ex:
To grant a system privilege to a user

GRANT  create  table  TO  hulk;

To grant more than one system privilege to only one user in a single Grant statement.

GRANT  create synonym,  create view,  create sequence  TO  hulk;

list of privileges must only contain system privileges and not any object privilege. This is because system privileges and object privileges cannot be granted together in a single grant command. 

To grant privileges to more than one user in single Grant Statement.

GRANT  create  procedure  TO  hulk,  batman;

With Admin Option
  you can use admin option flag only while granting system privilege and not with object privilege. 

Granting a system privilege with Admin Option Flag means that the grantee can grant or revoke the system privilege or role to or from any user or other role in the database. and can grant the system privilege or role with the ADMIN OPTION to any other user and role.

ex:GRANT  create trigger  TO  batman  WITH ADMIN OPTION;
On executing this query our user batman not only gets create trigger system privilege but can also grant, revoke, and drop the create trigger privilege to and from any user and roles.

2.Object Privileges With Grant Option 

Object privileges are the rights given to a user to access and perform some actions on the database objects or the objects owned by some other user in the database.

Every database object has a particular set of object privileges associated with it. Object privileges allow users to perform certain actions on database objects, for example Executing DML on tables.

-The owner has every object privilege on the Object owned by it.
-The privileges enjoyed by the Object owner cannot be revoked.
-The owner can grant any number of object privileges for that particular object to other database users.

If a user has ADMIN privilege then it can either grant or revoke object privileges to and from the users that are not the owners. 

The syntax for Grant DCL statement for granting Object Privilege is.

GRANT  object privilege name(s)  ON  object name  TO  user name(s)

The most commonly used object privileges are Select, Insert, Update, Delete and Execute.

For example say user Hulk wants to select the data from the employees table owned by HR user.

GRANT  select  ON  HR.Employees  TO  hulk;

 To grant multiple object privilege to a user / Grant All 

GRANT select, update, insert, delete, index, references ON hr.employees TO hulk;
or
GRANT ALL ON hr.employees TO hulk;

Object Privilege on column level

Oracle allows you to grant object privileges on column level also but you can only grant INSERT, UPDATE, and REFERENCES object privilege on column level.

WITH GRANT OPTION

Just like we use ADMIN OPTION flag with system privileges similarly we use Grant option flag with object privileges. GRANT OPTIONflag allows the user to grant an object privilege to another user.

GRANT  update  ON  hr.employees  TO  hulk  WITH GRANT OPTION;

After executing this query user Hulk not only gets update object privilege but can also grant and revoke the same privilege to and from any user or roles.

Roles

Usage of roles makes the managing and controlling of privileges an easy to handle process. Roles are named group of privileges that you can assign to users or other roles. Roles are the easiest and quickest way of granting permission to users.

-Every role within a database has to be unique with exclusive user names as well as role names.
-Roles are not like schema objects as they are not contained in any schema. This means that when a user who has created a role is dropped, this has no effect on the role.

Roles are often created by Database administrators for a given database application. In order for the application to run, you need to grant all necessary privileges to a secure application role. Furthermore you can even grant this secure application role to users or other roles. It is possible for an application to have several different roles, each of which in turn is granted different set of privileges. These set of privileges regulate the data access while the application is in usage. 

In order to prevent unauthorized usage of the privileges granted to a role, the DBA can secure it with a password at the time of the creation of that role. Usually an application is designed in such a way that it enables a proper role whenever started. Hence as a result there is no need for the application user to provide a password for an application role.

Advantages of Roles in Oracle Database

-Rather than assigning privileges one at a time directly to a user, you can create a role, assign privileges to that role, and then grant that role to multiple users and roles.
-When you add or delete a privilege from a role, all users and roles that are assigned the particular role automatically receive or lose that privilege.
-You can assign multiple roles to a user or role.
-You can assign a password to a role.

Roles can be used for granting permissions to users in a swift and easy way. Although one way is to use roles defined by the Oracle Database yet you can create your own roles with privileges pertaining to your requirements in order to get more control and continuity.

Syntax

CREATE ROLE name_role NOT IDENTIFIED| IDENTIFIED BY password | USING [schema.] package | EXTERNALLY| GLOBALLY;

Oracle recommends that the name of the role contain at least one single-byte character regardless of whether the database character set also contains multi-byte characters. The maximum number of user-defined roles that can be enabled for a single user at one time is 148. 

Then we have NOT IDENTIFIED clause. This is an option clause and indicates that this role is authorized by the database and that no password is required to enable the role. 
In case you want to provide security to your role to prevent unauthorized use of the privileges granted to the role, we have IDENTIFIED BY clause.

IDENTIFIED BY clause indicates that a role must be authorized by the specified method and helps the oracle engine in authenticating the role. In Oracle there are 4 ways of authenticating a role.

1.By Password: Password clause lets you create a local role and indicates that the user must specify password to the database when enabling the role. Remember here: passwords are case sensitive. Passwords can consist of only single-byte characters from your database character set regardless of whether the character set also has multi-byte characters
2.USING package: The USING package clause lets you create an application role, which is a role that can be enabled only by applications using an authorized package. If you do not specify schema, then the database assumes the package is of your own schema.
3.Externally: This clause helps you in creating an external role. These external roles and their authentication are managed by external services such as operating system for enabling the role.
4.Globally: Specify GLOBALLY to create a global role. A global user must be authorized to use the role by the enterprise directory service before the role is enabled at login.

If you omit both the NOT IDENTIFIED clause and the IDENTIFIED clause, then the role defaults to NOT IDENTIFIED.

ex:
-Role with NOT Identified clause

CREATE ROLE demo1 NOT IDENTIFIED;

 NOT IDENTIFIED clause indicates that this role is authorized by the database and no password is required to enable the role. 

-Role with Identified By Password clause

- External role.

 The authentication of external roles is handled by external services such as your Operating System.

CREATE USER Demo3 IDENTIFIED externally;

-Global role
 The authentication of global roles is handled by enterprise directory service before the role is enabled at login

CREATE USER Demo3 IDENTIFIED globally;

To assign privileges to a role a user must have GRANT ANY PRIVILEGE system privilege. You can grant Object privilege, system privilege as well as a role to a role. 

There are few things which you should take care of when granting roles. Such as:

-You cannot grant a role to itself
-A role cannot be granted circularly
-Though you can grant system privilege WITH ADMIN OPTION to a role but same is not true in case of Object Privileges means you cannot grant object privileges with GRANT OPTION to a role.

ex:
-How to grant System Privilege to a role( to the role Demo1)

GRANT create table TO Demo1 WITH ADMIN OPTION;

-How to grant Object Privilege to a Role

GRANT select ON hr. employees TO demo1;

-How to Grant a Role to another Role
Grant demo1 TO demo2;

Any user in the database could be granted a Role. However only the user with “GRANT ANY ROLE” can in turn grant or revoke any role to and from any user and given that role provided is not a GLOBAL role. Moreover a user when granted a role with ADMIN OPTION is also eligible to grant or revoke a role to and from any role or user.
 
you can grant any role to any roles except itself in the database.


Case Expressions

Using Case expression you can achieve the IF-THEN-ELSE logic in Oracle SQL that too without invoking any kind of SQL procedures. 

In Oracle there are two types of CASE expressions 

1.Simple case expressions
2.Searched case expressions

it is a flexible and easiest version of DECODE expression.
 
1.
The syntax of “Simple Case Expression” is 

CASE search_expression
  WHEN   input_expression 1   THEN   output_result 1
  WHEN   input_expression 2   THEN   output_result 2
  …
  WHEN   input_expression N   THEN   output_result N
  ELSE    Else_result
END

The Simple Case Expression uses search expression to determine the return value. Means simple case expression evaluates all the input expressions against the search expression. Once a match is found the corresponding result is returned otherwise Else result is displayed. Furthermore the Else statement is optional so in case you omit it then Null is displayed when the simple case expression cannot find a match.

Points to remember

The data type of all the input expressions must match with the data type of search expression. Means data type of search expression and input expression must be the same.
The datatype of all the output results must match with Else result means the data type of output result along with else result must be the same.

The maximum number of arguments in a CASE expression is 255. All expressions count toward this limit, including the initial expression of a simple CASE expression and the optional ELSE expression. Each WHEN … THEN pair counts as two arguments. To avoid exceeding this limit, you can nest CASE expressions so that the output_result itself is a CASE expression.

ex:
Query 1: Column Name as Search Expression.
SELECT first_name,department_id,
  (CASE    department_id
    WHEN    10    THEN    ‘Administration’
    WHEN    20    THEN    ‘Marketing’
    WHEN    30    THEN    ‘Purchasing’
    WHEN    40    THEN    ‘Shipping’
    WHEN    50    THEN    ‘Shipping’
    ELSE    ‘Sorry’
  END)    Departments
FROM    employees
WHERE    department_id    BETWEEN 10 AND 50
ORDER BY 2;

 We are using column department_id of employees table as our search expression and the values from department id column (10, 20, 30, 40 and 50) are serving as input expressions of WHEN-THEN pairs.

Whenever we specify a column name of a table as search expression the oracle engine treats all the data from this column as an array and matches all the input_expressions with every element of this array and if a match is found, it returns the corresponding result values, Otherwise the else statement gets return.

Query 2: String as Search Expression.
In case you specify a string instead of a column name as a search expression then oracle will find the first best fit match and then return the corresponding result value. 

SELECT
  CASE ‘Dog’
    WHEN    ‘Cat’    THEN    ‘1 Flase’
    WHEN    ‘Dog’    THEN    ‘2 True’
    WHEN    ‘Cow’    THEN    ‘3 False’
    WHEN    ‘Dog’    THEN    ‘4 True’
    ELSE    ‘Sorry’
  END
FROM    dual;

ex:
create table table1(id number,name varchar2(30));
insert into table1 values(11,'sathya');
select * from table1;
drop table table1;
select id ,name,
(case id 
  when 1 then 'first'
  when 2 then 'second'
  when 3 then 'third'
  when 4 then 'fourth'
  else 'none'
  end)as position
  from table1;

2.Searched Case Expression

Syntax

CASE 
  WHEN   condition 1   THEN   result 1
  WHEN   condition 2   THEN   result 2
  …
  WHEN   conditionN   THEN   result N

  ELSE    Else_result
END

ex:create table table1(id number,name varchar2(30));
insert into table1 values(11,'sathya');
select * from table1;
select id ,name,
(case  
  when id=1 and name='matysa' then 'vedas'
  when id=2 and name='kurma' then 'amrutham'
  when id=3 and name='varaha' then 'earth'
  when id=4 and name='narasimha' then 'prahlada'
  else 'love'
  end)as purpose
  from table1;


  Views

A logical table or a virtual table stored in a database
 also define a VIEW as SELECT Statement with a name which is stored in a database as though it were a table. All the DML commands which you can perform on a table can be performed on a view also.

Views in any database are majorly used for two reasons – 


1.Security
2.Reducing complexity of a query.

1.Security:
Using a VIEW you can mask columns of a table and restrict any user to use the data from that column. 

Suppose you have a large table containing a mixture of both sensitive as well as general interest information. In this case it will be very handy for you to create a view which only queries the general interest columns of the original table and in turn grants privileges on this view to the general users. In this case the population of general users can query the view and have direct access only to the general information omitting the sensitive information present in the underlying table. Thus in this way views can be used to give access to selective data in a table. 

Reducing the complexity of a query
Another reason for using a VIEW is to reduce the complexity of a query which is made by joining various tables. For example,

A view built on a complex join can be created in order to include the complexity in the view itself. The result of this is a regular view object that looks to be a single table which could be queried by you as any regular table. The view can be joined with other views and tables as well. Thus here in this way, view can be used to reduce the complexity of a common Join. 

CREATE VIEW  view_name  AS  
  SELECT  column_name(s)  FROM  table_name  WHERE condition;

We use OR REPLACE to recreate the definition of a view after creating it. 

CREATE  OR REPLACE  VIEW  vw_rebellion  AS 
  SELECT  first_name,  last_name,  email,  phone_number  FROM  employees; 

How to insert Row in a VIEW in Oracle Database (INSERT ddl)
To insert a row, a view has to satisfy all the constraints on any column of underlying table over which it’s created. Insert DML is subject to several restrictions when executed on a View in Oracle database such as:

A view must contain the entire Not Null column from underlying table. The failure to include any columns that have NOT NULL constraints from the underlying table in the view will result in the non-execution of the INSERT statement. However an UPDATE or DELETE statement can be issued.

If there is any column with referential key constraint in underlying table then view must contain that column and a proper value must be provided to that column while performing Insert dml on the View and many more.

Thus to perform an INSERT dml on a view, it has to include all the columns which either have NOT NULL constraint or Referential constraint or any other mandatory constraint from the base table employees.
E.g.

INSERT  INTO  vw_superheroes  VALUES  (‘Steve’, ‘Rogers’, 654765);

Where vw_supereheroes is another view created over Superheroes table which has 5 columns first name, last name, and real name, Phone number and SSN. This table has no constraint on any column.

How to create a complex view (View by joining two tables)
Complexity of a view depends upon the complexity of its SELECT statement. You can increase the complexity of a view by increasing the complexity of the SELECT statement. 

CREATE  OR  REPLACE  VIEW  vw_join  AS

  SELECT  first_name,  department_name  FROM  employees emp

   FULL  OUTER  JOIN  departments dept

  ON  (emp.DEPARTMENT_ID = dept.DEPARTMENT_ID);

As I told you above that by using a View you can reduce the complexity of a query. A view built on a complex join can be created in order to include the complexity in the view itself. The result of this is a regular view object that looks to be a single table which could be queried by you as any regular table. The view can be joined with other views and tables as well. Thus here in this way, view can be used to reduce the complexity of a common Join. 

Tablespace

Oracle database stores schema objects such as tables, indexes, Views etc. logically into the tablespace and physically in datafiles associated with the corresponding tablespace. So we can say that Tablespaces are logical storage units made up of one or more datafiles.

Datafiles

Datafiles are physical files stored on your disk created by Oracle database and has .dbf extension. These files are not only capable of storing tables but also indexes, synonyms, views and other schema objects created by you. These are physical files because they have existence on your OS’s file system and you can see them. These files are written by database writer (DBWR) processes and used by Oracle database for the proper functioning of your database system. I suggest you Do not try to modify these files manually. A datafile cannot be shared between multiple tablespaces which means that every datafile belongs to a specific tablespace.

Types of Tablespaces in Oracle Database

We can differentiate tablespaces on the basis of two factors:

1.Type of Data
2.Size of Data

Type of data consists of 3 kinds of tablespace including: 

1.Permanent Tablespace
2.Temporary Tablespace
3.Undo Tablespace

And on the basis of Size of Data we have 2 kinds of tablespace:

1.Big file tablespace
2.Small file tablespace

Permanent Tablespace In Oracle Database
It is the tablespace which contains persistent schema object which means the data stored in the permanent tablespace persists beyond the duration of a session or transaction. Objects in permanent tablespaces are stored in datafiles.

Temporary tablespace In Oracle Database
On the contrary, temporary tablespaces are the ones which contain schema objects only for the duration of a session which means that data stored in the temporary tablespace exists only for the duration of a session or a transaction. Objects in temporary tablespaces are stored in tempfiles.

Undo Tablespace In Oracle Database
Then there comes Undo tablespace. Undo tablespace is a special type of tablespace used by Oracle database to manage undo data if you are running your database in automatic undo management mode. Undo tablespace stores data permanently which means that undo tablespace are permanent in nature. Undo tablespace play a vital role in providing:

Read consistency for SELECT statements that access tables which in turn consist of rows which are in the process of being modified.
The ability to rollback a transaction that has failed to commit.
Next, we have Big-file tablespace and small file tablespace. 

Big-File tablespace In Oracle Database
The new concept started from Oracle 10g. Big file tablespace is best suited for storing large amounts of data. Big file tablespace can have a maximum of 1 datafile which means bigfile tablespaces are built on single data files which can be as big as 232 data blocks in size. So, a bigfile tablespace that uses 8KB data blocks can be as much as 32TB in size.

Small-File tablespace In Oracle Database
This is the default type of tablespace in Oracle database. Small file tablespace can have multiple datafiles and each datafile can be as much as 222 data blocks in size. A small file tablespace can have maximum up to 1022 data files but this number depends on your Operating system also.

You can create Permanent, temporary or undo tablespace either as big-file tablespace or small-file tablespace but by default they are always a small-file tablespace.

Q: What are the default tablespaces in Oracle Database?
A: The SYSTEM and SYSAUX tablespaces are always created when the database is created. One or more temporary tablespaces are usually created in a database along with an undo tablespace and several application tablespaces. Because SYSTEM and SYSAUX are the only tablespaces always created with the database, they are the default tablespaces.

Q: What type of tablespace do SYSTEM and SYSAUX belong to?
A: SYSTEM and SYSAUX are always created as SMALLFILE tablespace. 

1.Permanent tablespace
 contains persistent schema object which means that the data stored in the permanent tablespace persists beyond the duration of a session or transaction. Objects in permanent tablespaces are stored in data files.

To create a tablespace we use CREATE TABLESPACE ddl statement

CREATE SMALLFILE TABLESPACE rebellionrider
DATAFILE 
  ‘C:\app\TBSP_DEMO\Rebell1.dbf’ SIZE 100M, 
  ‘C:\app\TBSP_DEMO\Rebell2.dbf’ SIZE 100M 
LOGGING 
EXTENT MANAGEMENT LOCAL UNIFORM SIZE 100M 
SEGMENT SPACE MANAGEMENT AUTO;

To add the datafile you just have to write the name of the clause which is DATAFILE and then inside the single quote you either have to specify the name of datafile & the directory path (where you want to put your datafile) or you can simply write the name of your datafile. In latter case, oracle engine will create your datafile and place it in the default directory. 

After writing the name of your datafile you have to specify the size for your datafile using SIZE clause which is mandatory, if you forget to do so then oracle engine will give you an error. Let’s say  100MB as the size for my datafile rebel1. 

LOGGING CLAUSE:
The logging clause lets you specify whether creation of a database object will be logged in the redo log file (LOGGING) or not (NOLOGGING). Logging clause has two options: 

Logging and
Nologging
If you specify Logging then oracle engine will Generate redo logs for creation of tables, indexes and partitions, and for subsequent inserts. But in case you do not want to generate redo logs for all those operations which I just mentioned then simply write “NOLOGGING”. However logging is always better as it makes recovery much easier. 


creating an undo tablespace thus we will use CREATE UNDO TABLESPACE clause.

CREATE SMALLFILE UNDO TABLESPACE “tbsp_undo”
 DATAFILE ‘C:\APP\TBSP_DEMO\tbsp_undo.dbf’ SIZE 100M 
 AUTOEXTEND ON NEXT 500M 
 MAXSIZE UNLIMITED 
 RETENTION NOGUARANTEE;

How to Drop Undo Tablespace
To drop a tablespace from the database, we use the DROP TABLESPACE statement. The optional clause INCLUDING CONTENTS recursively removes any segments (tables, indexes, and so on) in the tablespace, like this:

DROP TABLESPACE dba_sandbox INCLUDING CONTENTS;

Dropping a tablespace does not automatically remove the datafiles from the file system. You need to use the additional clause INCLUDING CONTENTS AND DATAFILES to remove the underlying datafiles as well as the stored objects, like this:

DROP TABLESPACE hr_data INCLUDING CONTENTS AND DATAFILES;


Set operators allow us to combine rows returned from two or more queries. In SQL we have four set operators.

Union
Union All
Intersect
Minus

1.The UNION set operator returns results from all the participating queries after eliminating duplications. Let’s see the example

SELECT first_name, last_name FROM cricket
 UNION
 SELECT first_name, last_name FROM football;

2.Union All Set Operator
The UNION ALL operator returns results from all the participating queries, including the duplicate rows. This means that if we will execute the same query then we will get all the rows returned from both the tables along with the duplicate one.

SELECT first_name, last_name FROM cricket
 UNION ALL
 SELECT first_name, last_name FROM football;

Restrictions of Set Operators in Oracle Database

1.The expressions in the SELECT lists must match in number and data type.
2.Parentheses can be used to alter the sequence of execution.
3.The ORDER BY clause:
Can appear only at the very end of the statement
Will accept the column name, aliases from the first SELECT statement, or the positional notation
4.Duplicate rows are automatically eliminated except in UNION ALL.
5.Column names from the first query appear in the result.
6.The output is sorted in ascending order by default except in UNION ALL.

3.Intersect Set Operator
The INTERSECT operator returns rows that are common to both queries. Let’s see an example. 

SELECT first_name, last_name FROM cricket
 INTERSECT
 SELECT first_name, last_name FROM football;

On execution the resulting set will consist of only those rows that are common to both these tables.

4.The MINUS operator returns rows in the first query that are not present in the second query.

SELECT first_name, last_name FROM cricket
 MINUS
 SELECT first_name, last_name FROM football;

On execution the result set of this query will contain two rows from the cricket table which were not present in the football table.