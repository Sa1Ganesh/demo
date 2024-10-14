PL/SQL
SHOW user;
CREATE TABLE example (
 s_id number,
 s_name varchar2(30),
 s_place varchar2(30)
);
INSERT INTO example VALUES  (1,'sai','parthi');
INSERT INTO example  VALUES (2,'ram','ayodhya');
INSERT INTO example  VALUES (3,'krishna','dwaraka');
INSERT INTO example  VALUES (4,'ganesh','puttaparthi');
SELECT * from example;
--DROP TABLE example;
/*
SET SERVEROUTPUT ON;
DECLARE
BEGIN
DBMS_OUTPUT.PUT_LINE();
END;
*/
--VARIABLES
SET SERVEROUTPUT ON;
DECLARE
v_age number :=22;
BEGIN
DBMS_OUTPUT.PUT_LINE('age' ||' '|| v_age);
END;
/
SET SERVEROUTPUT ON;
DECLARE
v_name varchar2(30);
BEGIN
SELECT s_name INTO v_name from example where s_id=1 ;
DBMS_OUTPUT.PUT_LINE('name' ||' '|| v_name);
END;
/
--Anchored data types
--(syntax) variable_name typed-attribute%type
SET SERVEROUTPUT ON;
DECLARE
v_nam varchar2(30);
v_name v_nam%type;
--v_name example.s_name%type;
BEGIN
SELECT s_name INTO v_name from example where s_id=1 ;
DBMS_OUTPUT.PUT_LINE('name' ||' '|| v_name);
END;
/
--CONSTANTS
--constant_name CONSTANT datatype (data-width) :=  value;
DECLARE 
v_pi constant number :=3.14;
BEGIN
DBMS_OUTPUT.PUT_LINE('value of pi'|| ' '||v_pi);
END;
/
DECLARE 
v_pi constant number not null default 3.14;
BEGIN
DBMS_OUTPUT.PUT_LINE('value of pi'|| ' '||v_pi);
END;
/
--Bind variables a.k.a Host variables.
VARIABLE bind_v1 number;
VARIABLE bind_v2 varchar2(30);
VARIABLE;
VARIABLE bind_v1;
EXEC :bind_v1 :=5;
PRINT :bind_v1;
--print bind_v1;
BEGIN
:bind_v2 :='sai';
DBMS_OUTPUT.PUT_LINE(:bind_v2);
END;
/
SET AUTOPRINT ON;
VARIABLE bind_v3 number ;
EXEC :bind_v3 :=6;
/
--Conditional Control Statements 
--1.IF STATEMENTS
--i.IF THEN
/*
IF condition THEN
Statement1;
…
Statement N;
END IF;
*/

SET SERVEROUTPUT ON;
DECLARE
v_num number :=9;
BEGIN
IF v_num <10 THEN
DBMS_OUTPUT.PUT_LINE('value' || ' '||v_num);
END IF;
DBMS_OUTPUT.PUT_LINE('done');
END;
/
--ii.IF-THEN-ELSE 
/*
IF condition THEN
  Statement 1;
ELSE
  Statement 2;
END IF;
  Statement 3
*/
SET SERVEROUTPUT ON;
DECLARE
v_num number:=&enter_a_number;
BEGIN
IF mod(v_num,2)=0 THEN
DBMS_OUTPUT.PUT_LINE(v_num ||' '||'IS EVEN');
ELSE
DBMS_OUTPUT.PUT_LINE(v_num || ' '||'IS ODD');
END IF;
DBMS_OUTPUT.PUT_LINE('DONE');
END;
/
--iii.IF-THEN-ELSIF
/*
IF CONDITION 1 THEN
STATEMENT 1;
ELSIF CONDITION 2 THEN
STATEMENT 2;
ELSIF CONDITION 3 THEN
STATEMENT 3;
…
ELSE
STATEMENT N; 
END IF; 
*/

SET SERVEROUTPUT ON;
DECLARE
v_game varchar2(30) :='&enter_the_game';
BEGIN
IF v_game ='cricket' THEN
DBMS_OUTPUT.PUT_LINE('MSD');
ELSIF v_game ='badminton' THEN
DBMS_OUTPUT.PUT_LINE('LCW');
ELSE
DBMS_OUTPUT.PUT_LINE('none');
END IF;
END;
/
--2.CASE STATEMENTS
--i.Simple Case Expression.
/*
CASE search_expression
  WHEN   input_expression 1   THEN   output_result 1
  WHEN   input_expression 2   THEN   output_result 2
  …
  WHEN   input_expression N   THEN   output_result N
  ELSE    Else_result
END
*/
SELECT s_name,s_id ,
(case s_id 
when 1 then 'one'
when 2 then 'two'
when 3 then 'three'
else 'none'
end)check_id from example where s_id between 1 and 3;
--ii.Searched Case Expression
/*
CASE 
  WHEN   condition 1   THEN   result 1
  WHEN   condition  2   THEN   result 2
  …
  WHEN   condition  N   THEN   result N
   ELSE    Else_result
END
*/
SELECT s_name,s_id ,
(case
when s_id=1 then 'one'
when s_id=2 then 'two'
when s_id=3 then 'three'
else 'none'
end)check_id from example where s_id between 1 and 3;
--DECODE
--decode (value,search_value ,result,default_value)
select s_name,
decode(s_name ,
'sai','sa1',
'ram','r@m',
'not same') name from example;
-- Loops 
--i.Simple Loop
/*
LOOP
Statement 1;
Statement 2;
…
Statement 3;
END LOOP;
*/
SET SERVEROUTPUT ON;
DECLARE
v_num number :=0;
v_mul number:=&enter_a_num;
v_result number;
BEGIN
LOOP
v_num :=v_num+1;
v_result :=v_num*v_mul;
DBMS_OUTPUT.PUT_LINE( v_mul || 'x'|| v_num ||'='||v_result);
exit when v_num>=10;
END LOOP;
END;
/
--ii.WHILE Loop 
/*
WHILE condition LOOP
Statement 1;
Statemen 2;
…
Statement 3;
END LOOP;
*/
SET SERVEROUTPUT ON;
DECLARE
v_num number:=1;
v_result number;
v_mul number:=&enter_a_num;
BEGIN
WHILE v_num<=10
LOOP
v_result :=v_mul*v_num;
DBMS_OUTPUT.PUT_LINE(v_mul||'x'||v_num||'='||v_result);
v_num:=v_num+1;
END LOOP;
DBMS_OUTPUT.PUT_LINE('while loop');
END;
/
--iii.Numeric FOR loop
/*
FOR loop_counter IN [REVERSE] lower limit.. upper_limit LOOP
  Statement 1;
  Statement 2;
  …
  Statement 3;
END LOOP;
*/
SET SERVEROUTPUT ON;
DECLARE
v_result number;
v_mul number:=&enter_a_num;
BEGIN
FOR v_num in 1..10
LOOP
v_result :=v_mul*v_num;
DBMS_OUTPUT.PUT_LINE(v_mul||'x'||v_num||'='||v_result);
END LOOP;
DBMS_OUTPUT.PUT_LINE('while loop');
END;
/
--TRIGGERS
/*
CREATE [OR REPLACE] TRIGGER Ttrigger_name
{BEFORE|AFTER} Triggering_event ON table_name
[FOR EACH ROW]
[FOLLOWS another_trigger_name]
[ENABLE/DISABLE]
[WHEN condition]
DECLARE
  declaration statements
BEGIN
  executable statements
EXCEPTION
  exception-handling statements
END;
*/
--i.DML Triggers
CREATE TABLE table_1 (t_name varchar2(30));
--drop table table_1;
SET SERVEROUTPUT ON;
CREATE TRIGGER trig 
before insert or update or delete on table_1
for each row 
enable
declare 
v_user varchar2(30);
BEGIN
select user into v_user from dual;
IF INSERTING THEN
DBMS_OUTPUT.PUT_LINE('INSERTED'||v_user);
ELSIF DELETING THEN
DBMS_OUTPUT.PUT_LINE('DELETED'||v_user);
ELSIF UPDATING THEN
DBMS_OUTPUT.PUT_LINE('UPDATED'||v_user);
END IF;
END;
/
INSERT INTO table_1 values('sai');
delete from table_1;
alter trigger trig disable;
drop trigger trig;
--Table Auditing Using DML Triggers
CREATE TABLE t1_audit(
  new_name varchar2(30),
  old_name varchar2(30),
  user_name varchar2(30),
  entry_date varchar2(30),
  operation  varchar2(30)
);
select * from t1_audit;
--DROP TABLE t1_audit;
create or replace trigger trig 
before insert or update or delete on table_1
for each row
enable
DECLARE
v_user varchar2(30);
v_date varchar2(30);
BEGIN
select user ,to_char(sysdate,'dd/mm/yyyy hh24:mi:ss:') into v_user ,v_date from dual;
IF INSERTING THEN
INSERT INTO t1_audit VALUES(:new.t_name,NULL,V_USER,V_DATE,'INSERT');
ELSIF DELETING THEN
INSERT INTO t1_audit VALUES(NULL,:old.t_name,V_USER,V_DATE,'DELETE');
ELSIF UPDATING THEN
INSERT INTO t1_audit VALUES(:new.t_name,:old.t_name,V_USER,V_DATE,'UPDATE');
END IF;
END;
/
insert into table_1 values('sai');
update table_1 set t_name='sathya';
delete from table_1;
--synchronized backup copy of a table
desc table_1;
create table tablebackup_1 as select * from table_1 ;
select * from tablebackup_1;
drop table tablebackup_1;
create or replace trigger trig 
before insert or delete or update on table_1
for each row 
enable
BEGIN
IF INSERTING THEN 
insert into tablebackup_1 values(:new.t_name);
ELSIF DELETING THEN
delete from tablebackup_1 where t_name=:old.t_name;
ELSIF UPDATING THEN 
update tablebackup_1 set t_name =:new.t_name where t_name=:old.t_name;
END IF;
END;
/
--ii.DDL Triggers
--Schema & Database Auditing 
CREATE TABLE schema_audit
  (
    ddl_date       DATE,
    ddl_user       VARCHAR2(15),
    object_created VARCHAR2(15),
    object_name    VARCHAR2(15),
    ddl_operation  VARCHAR2(15)
  );
select * from schema_audit;
drop table schema_audit;
create or replace trigger trig 
after ddl on schema
begin
insert into schema_audit values(sysdate,sys_context('USERENV','CURRENT_USER'),ora_dict_obj_type,ora_dict_obj_name,ora_sysevent);
end;
/
create or replace trigger trig 
after ddl on DATABASE
begin
insert into schema_audit values(sysdate,sys_context('USERENV','CURRENT_USER'),ora_dict_obj_type,ora_dict_obj_name,ora_sysevent);
end;
/
CREATE TABLE SAMP (C_1 NUMBER);
DROP TABLE SAMP;
--iii.Database Event Triggers.
/*
CREATE OR REPLACE TRIGGER trigger_name
BEFORE | AFTER database_event ON database/schema
BEGIN
	PL/SQL Code
END;
/
*/
CREATE TABLE event_audit
  (
    event_type VARCHAR2(30),
    logon_date DATE,
    logon_time VARCHAR2(15),
    logof_date DATE,
    logof_time VARCHAR2(15)
  );
select * from event_audit;
create or replace  trigger   trig
after logon on schema 
begin
insert into event_audit values(
ora_sysevent,
sysdate,
to_char(sysdate,'hh24:mi:ss'),
null,
null
);
end;
/
disc;
conn system/sathya;
create or replace  trigger   trig
before logoff on schema 
begin
insert into event_audit values(
ora_sysevent,
null,
null,
sysdate,
to_char(sysdate,'hh24:mi:ss')
);
end;
/
-- Database event Startup and Shutdown Triggers.
CREATE TABLE startup_audit 
(
  Event_type  VARCHAR2(15),
  event_date  DATE,
  event_time  VARCHAR2(15)
);
select * from startup_audit ;
CREATE OR REPLACE TRIGGER startup_audit
AFTER STARTUP ON DATABASE
BEGIN
  INSERT INTO startup_audit VALUES
(
    ora_sysevent,
    SYSDATE,
    TO_CHAR(sysdate, 'hh24:mm:ss')
  );
END;
/
drop trigger startup_audit;
CREATE OR REPLACE TRIGGER tr_shutdown_audit
BEFORE SHUTDOWN ON DATABASE
BEGIN
  INSERT INTO startup_audit VALUES(
    ora_sysevent,
    SYSDATE,
    TO_CHAR(sysdate, 'hh24:mm:ss')
  );
END;
/
drop trigger tr_shutdown_audit;
--iv.Instead-Of Insert Triggers
/*
CREATE [OR REPLACE] TRIGGER trigger_name
INSTEAD OF operation
ON view_name
FOR EACH ROW
BEGIN
	---Your SQL Code—
END;
/
*/
CREATE TABLE trainer
  ( 
    full_name VARCHAR2(20)
  );
select * from trainer;
--drop table  trainer;
CREATE TABLE subject
  ( 
    subject_name VARCHAR2(15)
  );
select * from subject;
--drop table subject;

INSERT INTO trainer VALUES ('Manish Sharma');
INSERT INTO subject VALUES ('Oracle');

CREATE VIEW vw_rebellionrider AS
SELECT full_name, subject_name FROM trainer, subject;
select * from vw_rebellionrider;
--drop view vw_rebellionrider;

CREATE OR REPLACE TRIGGER tr_Io_Insert
INSTEAD OF INSERT ON vw_rebellionrider
FOR EACH ROW
BEGIN
  INSERT INTO trainer (full_name) VALUES (:new.full_name);
  INSERT INTO subject (subject_name) VALUES (:new.subject_name);
END;
/
insert into vw_rebellionrider values('sai','cricket');
drop trigger tr_Io_Insert;
--CURSORS
/*
STEP1:Declare
CURSOR cursor_name IS select_statement;
STEP2:Open
OPEN cursor_name;
STEP3:Fetch
FETCH cursor_name INTO PL/SQL variable;
Or 
FETCH cursor_name INTO PL/SQL record;
STEP4:Close
CLOSE cursor_name;

EX:
DECLARE
	CURSOR cursor_name IS select_statement; 
BEGIN
	 OPEN cursor_name;
	 FETCH cursor_name INTO PL/SQL variable [PL/SQL record]; 
CLOSE cursor_name; 
END;
/
Cursor Attributes
%FOUND
%NOTFOUND
%ISOPEN
%ROWCOUNT
*/
SELECT * from example;
SET SERVEROUTPUT ON;
DECLARE
v_name varchar2(30);
CURSOR cur_n IS 
select s_name from example where s_id<5;
BEGIN
OPEN cur_n;
loop
FETCH cur_n into v_name ;
dbms_output.put_line(v_name);
exit when cur_n%notfound;
end loop;
END;
/
--Parameterized cursor
/*
CURSOR cur _ name (parameter list) IS SELECT statement;
OPEN cur _ name (argument list)
*/
SET SERVEROUTPUT ON;
DECLARE
v_name varchar2(30);
CURSOR cur_n(var_id varchar2)IS 
select s_name from example where s_id<var_id;
BEGIN
OPEN cur_n(4);
loop
FETCH cur_n into v_name ;
dbms_output.put_line(v_name);
exit when cur_n%notfound;
end loop;
END;
/
DECLARE
v_name varchar2(30);
v_place varchar2(30);
CURSOR cur_n(var_id varchar2:=4)IS 
select s_name,s_place from example where s_id<var_id;
BEGIN
OPEN cur_n(3);
loop
FETCH cur_n into v_name,v_place ;
dbms_output.put_line(v_name||' '||v_place);
exit when cur_n%notfound;
end loop;
close cur_n;
END;
/
--Cursor For Loop
/*
DECLARE
	CURSOR cursor_name IS select_statement; 
BEGIN    
FOR loop_index IN cursor_name 
	LOOP
		Statements…
	END LOOP;
END;
*/
DECLARE
CURSOR cur_n(var_id varchar2) IS 
select s_name ,s_place from example where s_id<var_id;
BEGIN
FOR LIDX in cur_n(4)
loop
dbms_output.put_line(lidx.s_name||' '||lidx.s_place);
end loop;
END;
/
--RECORDS
--i.Table Based Record
/*
Variable_ name   table_name%ROWTYPE;
-To Access data from record variable.
Record_variable_name.column_name_of_the_table; 
*/
DECLARE
v_exa example%rowtype;
BEGIN
select * into v_exa from example where s_id=1;
dbms_output.put_line(v_exa.s_name||' '||v_exa.s_place);
END;
/
DECLARE
v_exa example%rowtype;
BEGIN
select s_name,s_place  into v_exa.s_name,v_exa.s_place from example where s_id=1;
dbms_output.put_line(v_exa.s_name||' '||v_exa.s_place);
END;
/
--ii.Cursor based Record
/*
Variable_ name   cursor_name%ROWTYPE;
*/
set serveroutput on;
DECLARE
CURSOR cur_n IS 
select s_name,s_place from example where s_id<4;
v_exa cur_n%rowtype;
BEGIN
OPEN cur_n;
loop
FETCH cur_n into v_exa;
dbms_output.put_line(v_exa.s_name||' '||v_exa.s_place);
exit when cur_n%notfound;
end loop;
close cur_n;
END;
/
--iii.User defined Record 
/*
TYPE type_name IS RECORD (
field_name1 datatype 1,
field_name2 datatype 2,
...
field_nameN datatype N 
);

record_name TYPE_NAME;
*/
set serveroutput on;
DECLARE
type type_n is record (
v_n varchar2(30),
v_p varchar2(30)
);
var_e type_n;
BEGIN
select s_name ,s_place into var_e.v_n ,var_e.v_p from example where s_id=1;
dbms_output.put_line(var_e.v_n||' '||var_e.v_p);
END;
/
--Functions 
/*
CREATE [OR REPLACE] FUNCTION function_name
(Parameter 1, Parameter 2…)
RETURN datatype
IS
	Declare variable, constant etc.  
BEGIN
	Executable Statements
	Return (Return Value);
END;
*/
create or replace function c_area (radius number)
return number is
pi constant number :=3.141;
area number;
BEGIN
area:=pi*(radius * radius);
return area;
END;
/
set serveroutput on;
declare 
var_a number;
begin
var_a:=c_area(5);
dbms_output.put_line(var_a);
--dbms_output.put_line(c_area(5));
end;
/
--PROCEDURES
/*
CREATE [OR REPLACE] PROCEDURE pro_name (Parameter – List)
IS [AUTHID 	DEFINER | CURRENT_USER]
	Declare statements
BEGIN
	Executable statements 
END procedure name;
/ 
*/
create or replace procedure proc_n is
var_name varchar2(30):='sai ganesh';
var_fav varchar2(30):='cricket';
BEGIN
dbms_output.put_line ('myself' ||' '||var_name ||' '||'my fav game is'||' '||var_fav);
END;
/
--calling 
exec proc_n;
begin
proc_n;
end;
/
select * from example;
alter table example add salary number;
update example set salary =45000;
create or replace procedure proc_n(d_id number, sal_raise number) is
BEGIN
update example set salary =salary*sal_raise where s_id=d_id;
END;
/
exec proc_n(2,2);
execute proc_n(3,4);
begin
proc_n(1,3);
end;
/
-- Calling Notations
CREATE OR REPLACE FUNCTION add_num
(var_1 NUMBER, var_2 NUMBER DEFAULT 0, var_3 NUMBER ) RETURN NUMBER 
IS
BEGIN
  RETURN var_1 + var_2 + var_3;
END;
/
--Named Calling Notation
DECLARE
  var_result  NUMBER;
BEGIN
  var_result := add_num(var_3 => 5, var_1 =>2);
  DBMS_OUTPUT.put_line('Result ->' || var_result);
END;
/
-- Mixed calling notation
DECLARE
  var_result  NUMBER;
BEGIN
  var_result := add_num(var_1 => 10, 30 ,var_3 =>19);
  DBMS_OUTPUT.put_line('Result ->' || var_result);
END;
/
--Packages
/*
Package Specification

CREATE OR REPALCE PACKAGE pkg_name IS
	Declaration of all the package element…;
END [pkg_name]; 

Package Body

CREATE OR REPALCE PACKAGE BODY pkg_name IS
	Variable declaration;
	Type Declaration;
BEGIN
	Implementation of the package elements…
END [pkg_name];
*/
create or replace package pkg is 
FUNCTION p_strng RETURN VARCHAR2;
procedure proc_n(d_id number, sal_raise number);
end pkg;
/
create or replace package body pkg is
FUNCTION p_strng RETURN VARCHAR2 is 
BEGIN 
return 'package';
END p_strng;
procedure proc_n(d_id number, sal_raise number) is
BEGIN
update example set salary =salary*sal_raise where s_id=d_id;
END;
END pkg;
/
exec proc_n(4,2);
begin
 DBMS_OUTPUT.PUT_LINE (pkg.p_strng);
end;
/
-- Exception Handling 
--i.User-Define Exception Using An Exception Variable
set serveroutput on;
DECLARE
var_n number :=&enter_number;
var_d number :=&enter_divisor;
var_r number;
ex_divzero EXCEPTION;
BEGIN
IF var_d =0 THEN
RAISE ex_divzero;
end if;
var_r :=var_n/var_d;
DBMS_OUTPUT.PUT_LINE('Result = ' ||var_r);
EXCEPTION when ex_divzero THEN
DBMS_OUTPUT.PUT_LINE('error - divisor is zero');
END;
/
--ii.User-Define Exception Using RAISE_APPLICATION_ERROR
/*
raise_application_error (error_number, message [, {TRUE | FALSE}]);
*/
SET SERVEROUTPUT ON;
ACCEPT var_age NUMBER PROMPT 'What is yor age';
DECLARE
  age   NUMBER := &var_age;
BEGIN
  IF age < 18 THEN
    RAISE_APPLICATION_ERROR (-20008, 'you should be 18 or above for the DRINK!');
  END IF; 
  DBMS_OUTPUT.PUT_LINE ('Sure, What would you like to have?'); 
  EXCEPTION WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE (SQLERRM);
END;
/ 
--iii.User Define Exception Using PRAGMA EXCEPTION_INIT
DECLARE
  ex_age    EXCEPTION;
  age       NUMBER    := 17;
  PRAGMA EXCEPTION_INIT(ex_age, -20008);
BEGIN
  IF age<18 THEN
    RAISE_APPLICATION_ERROR(-20008, 'You should be 18 or above for the drinks!');
  END IF;
  
  DBMS_OUTPUT.PUT_LINE('Sure! What would you like to have?');
  
  EXCEPTION WHEN ex_age THEN
    DBMS_OUTPUT.PUT_LINE(SQLERRM);   
END;
/
--COLLECTIONS
--i.Nested Table
/*
DECLRE 
TYPE nested_table_name IS TABLE OF element_type [NOT NULL];
*/
set serveroutput on;
DECLARE
type nested_table is table of number;
var_nt nested_table := nested_table(10,20,30,40,50,60);
BEGIN
DBMS_OUTPUT.PUT_LINE('VALUE AT INDEX 1 IN NT IS '||var_nt(1));
DBMS_OUTPUT.PUT_LINE('VALUE AT INDEX 2 IN NT IS '||var_nt(2));
DBMS_OUTPUT.PUT_LINE('VALUE AT INDEX 3 IN NT IS '||var_nt(3));
END;
/
set serveroutput on;
DECLARE
type nested_table is table of number;
var_nt nested_table := nested_table(10,20,30,40,50,60);
BEGIN
FOR i in 1..var_nt.count
loop
DBMS_OUTPUT.PUT_LINE('VALUE AT INDEX i IN NT IS '||var_nt(i));
end loop;
END;
/
--Nested Table As Database Object
set serveroutput on;
create or replace type nested_t is table of varchar2(10);
/
create table my_subject(
sub_id    	NUMBER,
sub_name  	VARCHAR2(20),
sub__day    nested_t
)NESTED TABLE sub_day STORE AS sub_d;
/
--Nested Table Using User-Define Datatype
CREATE OR REPLACE TYPE object_type AS OBJECT (
  obj_id  NUMBER,
  obj_name  VARCHAR2(10)
);
/
CREATE OR REPLACE TYPE My_NT IS TABLE OF object_type;
/
CREATE TABLE Base_Table(
  tab_id  NUMBER,
  tab_ele My_NT
)NESTED TABLE tab_ele STORE AS stor_tab_1;
/
--ii.VARRAYs 
/*
--VARRAY as Database Object

CREATE [OR REPLACE] TYPE type_name
IS {VARRAY | VARYING ARRAY} (size_limit) OF element_type;

--VARRAY as a member of PL/SQL Block

DECLARE
TYPE type_name IS {VARRAY | VARYING ARRAY} (size_limit) OF
element_type;

ALTER TYPE type_name MODIFY LIMIT new-size-limit [INVALIDATE | CASCADE]

DROP TYPE type_name [FORCE];
*/
--VARRAYs As PL/SQL Block Member 
SET SERVEROUTPUT ON;
DECLARE
TYPE inBlock_vry IS VARRAY (5) OF NUMBER;
vry_obj inBlock_vry  :=  inBlock_vry();
BEGIN
		vry_obj.EXTEND(5); 
		vry_obj(1):= 10*2;
		DBMS_OUTPUT.PUT_LINE(vry_obj(1));    
END;
/
SET SERVEROUTPUT ON;
DECLARE
TYPE inBlock_vry IS VARRAY (5) OF NUMBER;
vry_obj inBlock_vry  :=  inBlock_vry();
BEGIN
FOR i IN 1 .. vry_obj.LIMIT
LOOP
 		vry_obj.EXTEND;
		vry_obj (i):= 10*i;    
		DBMS_OUTPUT.PUT_LINE (vry_obj (i));    
END LOOP;
END;
/
--VARRAYs As Database Object
SET SERVEROUTPUT ON;
CREATE OR REPLACE TYPE dbObj_vry IS VARRAY (5) OF NUMBER;
/
CREATE TABLE calendar(
    day_name        VARCHAR2(25),
    day_date        dbObj_vry
);
/
INSERT INTO calendar ( day_name, day_date ) 
VALUES ( 'Sunday', dbObj_vry (7, 14, 21, 28) ); 
DECLARE
    vry_obj dbObj_vry    := dbObj_vry();
BEGIN
    FOR i IN 1..vry_obj.LIMIT
    LOOP
        vry_obj.EXTEND; 
        vry_obj(i):= 10*i;
        DBMS_OUTPUT.PUT_LINE(vry_obj(i));    
    END LOOP;
END;
/
--iii. Associative Arrays 
/*
TYPE aArray_name IS TABLE OF element_datatype [Not Null]
INDEX BY index_elements_datatype;
*/
SET SERVEROUTPUT ON;
DECLARE
    TYPE books IS TABLE OF NUMBER
        INDEX BY VARCHAR2(20);
    isbn Books;
BEGIN
-- How to insert data into the associative array     	
isbn('Oracle Database') := 1234;
isbn('MySQL') := 9876;
DBMS_OUTPUT.PUT_LINE('Value Before Updation '||isbn('MySQL'));
    
-- How to update data of associative array.
isbn('MySQL') := 1010;
    
-- how to retrieve data using key from associative array.  
DBMS_OUTPUT.PUT_LINE('Value After Updation '||isbn('MySQL'));
END;
/
--Collection Methods
/*
Collection Functions
Count(returns the number of non-empty elements)
Exists(checks the existence of an element at a specific index in a collection)
First, Last(three types of collections)( to know the first and last index values defined in a collection.)
Limit( returns the maximum number of elements that a VARRAY can hold. )
Prior, Next(three types of collections)

Collection Procedures
Delete(three types of collections)
Extend(nested table and varrays)
Trim(nested table and varrays)

Collection. Method (parameters) 

*/
--i.COUNT Function
SET SERVEROUTPUT ON;
DECLARE
    TYPE my_nested_table IS TABLE OF number;
    var_nt my_nested_table := my_nested_table (9,18,27,36,45,54,63,72,81,90);
BEGIN
    DBMS_OUTPUT.PUT_LINE ('The Size of the Nested Table is ' ||var_nt.count);
END;
/
--ii.EXISTS Function(does not raise any exception)
--EXISTS (index number);
SET SERVEROUTPUT ON;
DECLARE
        --Declare a local Nested Table
    	TYPE my_nested_table IS TABLE OF VARCHAR2 (20);
 --Declare collection variable and initialize the collection.	
col_var_1   my_nested_table := my_nested_table('Super Man','Iron Man','Bat Man');
BEGIN
IF col_var_1.EXISTS (1) THEN
        DBMS_OUTPUT.PUT_LINE ('Hey we found '||col_var_1 (1));
ELSE
        DBMS_OUTPUT.PUT_LINE ('Sorry, no data at this INDEX');
END IF;
END;
/  
--iii and iv.FIRST & LAST Functions
SET SERVEROUTPUT ON;
DECLARE
    TYPE nt_tab IS TABLE OF NUMBER;
    col_var nt_tab := nt_tab(10, 20, 30, 40, 50);
BEGIN
    DBMS_OUTPUT.PUT_LINE ('First Index of the Nested table is ' || col_var.FIRST);
    DBMS_OUTPUT.PUT_LINE ('Last Index of the Nested table is ' || col_var.LAST);
END;
/
SET SERVEROUTPUT ON;
DECLARE
    TYPE nt_tab IS TABLE OF NUMBER;
    col_var nt_tab := nt_tab(10, 20, 30, 40, 50);
BEGIN
col_var.DELETE(1);
    DBMS_OUTPUT.PUT_LINE ('First Index after DELETE is ' || col_var.FIRST);
END;
/
SET SERVEROUTPUT ON;
DECLARE
    TYPE nt_tab IS TABLE OF NUMBER;
    col_var nt_tab := nt_tab(10, 20, 30, 40, 50);
BEGIN
-- This output statement will return 10 which is the value stored at the first index
DBMS_OUTPUT.PUT_LINE ('Value at First Index '|| col_var(col_var.FIRST));
-- This output statement will return 50 which is the value stored at the last index
DBMS_OUTPUT.PUT_LINE ('Value at First Index '|| col_var(col_var.LAST));
END;
/
--v.LIMIT Function
SET SERVEROUTPUT ON;
DECLARE
    TYPE inBlock_vry IS VARRAY (5) OF NUMBER;
    vry_obj inBlock_vry := inBlock_vry();
BEGIN
 --Let's find out total number of indexes in the above VARRAY
    DBMS_OUTPUT.PUT_LINE ('Total Indexes '||vry_obj.LIMIT);
END;
/
SET SERVEROUTPUT ON;
DECLARE
    --Create VARRAY of 5 element
    TYPE inblock_vry IS
        VARRAY ( 5 ) OF NUMBER;
    vry_obj   inblock_vry := inblock_vry ();
BEGIN
    --Insert into VARRAY
    	vry_obj.extend;
    	vry_obj(1) := 10 * 2; 
dbms_output.put_line('Total Number of Index ' || vry_obj.limit);
dbms_output.put_line('Total Number of Index which are occupied ' || vry_obj.count);
END;
/
--vi and vii.PRIOR & NEXT Functions
SET SERVEROUTPUT ON;
DECLARE
TYPE my_nested_table IS
TABLE OF NUMBER;
var_nt   my_nested_table := my_nested_table(9,18,27,36,45,54,63,72,81,90);
BEGIN
dbms_output.put_line('Index prior to index 3 is '||var_nt.PRIOR(3)); 
dbms_output.put_line('Value before 3rd Index is '||var_nt(var_nt.PRIOR(3))); 
END;
/
DECLARE
TYPE my_nested_table IS
TABLE OF NUMBER;
var_nt   my_nested_table := my_nested_table(9,18,27,36,45,54,63,72,81,90);
BEGIN
var_nt.DELETE(2);
dbms_output.put_line('Index prior to index 3 is '||var_nt.PRIOR(3)); 
dbms_output.put_line('Value before 3rd Index is '||var_nt(var_nt.PRIOR(3))); 
END;
/
--DELETE Procedure(overloaded procedure)
--i.Simple procedure call without argument.
DECLARE
TYPE my_nested_table IS
TABLE OF NUMBER;
 var_nt my_nested_table := my_nested_table(2,4,6,8,10,12,14,16,18,20);
BEGIN
var_nt.DELETE;
FOR i IN 1..var_nt.LAST LOOP
IF var_nt.EXISTS(i) THEN
DBMS_OUTPUT.PUT_LINE('Value at Index ['||i||'] is '|| var_nt(i));
END IF;
END LOOP;
END;
/
--ii.Procedure call with a single parameter
DECLARE
TYPE my_nested_table IS
TABLE OF NUMBER;
var_nt my_nested_table := my_nested_table(2,4,6,8,10,12,14,16,18,20);
BEGIN
DBMS_OUTPUT.PUT_LINE('After Deleted');
--Delete Specific Index
var_nt.DELETE(5);
IF var_nt.EXISTS(5) THEN
DBMS_OUTPUT.PUT_LINE('Value at Index [5] is '|| var_nt(5));
ELSE
DBMS_OUTPUT.PUT_LINE('Data is Deleted');
END IF;
END;
/
--iii. Procedure call with two parameters.
DECLARE
TYPE my_nested_table IS
TABLE OF NUMBER;
var_nt my_nested_table := my_nested_table(2,4,6,8,10,12,14,16,18,20);
BEGIN
--Delete Range
var_nt.DELETE(2,6);
FOR i IN 1..var_nt.LAST LOOP
IF var_nt.EXISTS(i) THEN
DBMS_OUTPUT.PUT_LINE('Value at Index ['||i||'] is '|| var_nt(i));
END IF;
END LOOP;
END;
/
-- EXTEND Procedure 
--PL/SQL Collection Procedure EXTEND with No Argument.
SET SERVEROUTPUT ON;
DECLARE
TYPE my_nestedTable IS TABLE OF number;
nt_obj  my_nestedTable := my_nestedTable();
BEGIN
 nt_obj.EXTEND;
 nt_obj(1) := 10;
DBMS_OUTPUT.PUT_LINE ('Data at index 1 is '||nt_obj(1));
END;
/
--Collection Procedure EXTEND with One Argument.
SET SERVEROUTPUT ON;
DECLARE
    TYPE my_nestedTable IS TABLE OF number;
    nt_obj  my_nestedTable := my_nestedTable();
BEGIN
    nt_obj.EXTEND(3);
    nt_obj(1) := 10;
    nt_obj(2) := 20;
    nt_obj(3) := 30;
    DBMS_OUTPUT.PUT_LINE ('Data at index 1 is '||nt_obj(1));
    DBMS_OUTPUT.PUT_LINE ('Data at index 2 is '||nt_obj(2)); 
    DBMS_OUTPUT.PUT_LINE ('Data at index 3 is '||nt_obj(3));
END;
/
--PL/SQL Collection Procedure EXTEND with Two Arguments.
SET SERVEROUTPUT ON;
DECLARE
    TYPE my_nestedTable IS TABLE OF number;
    nt_obj  my_nestedTable := my_nestedTable();
BEGIN
    nt_obj.EXTEND;
    nt_obj(1) := 28;
    DBMS_OUTPUT.PUT_LINE ('Data at index 1 is '||nt_obj(1));
    nt_obj.EXTEND(5,1);
    DBMS_OUTPUT.PUT_LINE ('Data at index 4 is '||nt_obj(4));
END;
/
--Trim Procedure
--PL/SQL Collection Procedure TRIM without parameter.
SET SERVEROUTPUT ON;
DECLARE
    TYPE my_nestedTable IS TABLE OF number;
    nt_obj  my_nestedTable := my_nestedTable(1,2,3,4,5);
BEGIN
    nt_obj.TRIM;
    DBMS_OUTPUT.PUT_LINE ('After TRIM procedure');
    FOR i IN 1..nt_obj.COUNT
    LOOP
        DBMS_OUTPUT.PUT_LINE (nt_obj (i));
    END LOOP;
END;
/ 
--Collection Procedure TRIM with parameter.
SET SERVEROUTPUT ON;
DECLARE
    TYPE my_nestedTable IS TABLE OF number;
    nt_obj  my_nestedTable := my_nestedTable(1,2,3,4,5);
BEGIN
    nt_obj.TRIM (3);
    DBMS_OUTPUT.PUT_LINE ('After TRIM procedure');
    FOR i IN 1..nt_obj.COUNT
    LOOP
        DBMS_OUTPUT.PUT_LINE (nt_obj(i));
    END LOOP;
END;
/
SET SERVEROUTPUT ON;
DECLARE
 TYPE inBlock_vry IS VARRAY (5) OF NUMBER;
 vry_obj inBlock_vry := inBlock_vry(1, 2, 3, 4, 5);
BEGIN
    --TRIM without parameter
    vry_obj.TRIM;
    DBMS_OUTPUT.PUT_LINE ('After TRIM procedure');
    FOR i IN 1..vry_obj.COUNT
    LOOP
        DBMS_OUTPUT.PUT_LINE (vry_obj(i));
    END LOOP;
    --TRIM with Parameter
    vry_obj.TRIM (2);
    DBMS_OUTPUT.PUT_LINE ('After TRIM procedure');
    FOR i IN 1..vry_obj.COUNT
    LOOP
        DBMS_OUTPUT.PUT_LINE (vry_obj(i));
    END LOOP;
END;
/