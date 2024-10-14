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