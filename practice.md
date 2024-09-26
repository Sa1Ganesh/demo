PL/SQL
SHOW user;
SELECT * FROM student;
DESC student;
--pl/sql
SET SERVEROUTPUT ON;
DECLARE
v_name VARCHAR2(30);
BEGIN
SELECT s_name  INTO v_name FROM student WHERE s_id=1  ;
DBMS_OUTPUT.PUT_LINE('STUDENT NAME' ||' '||v_name);
END;
/
--variables
SET SERVEROUTPUT ON;
DECLARE 
var1 number;
var var1%type:=10;
--var number;
BEGIN
--var:=10;
DBMS_OUTPUT.PUT_LINE(var);
END;
/
--anchored datatype
DECLARE
v_name student.s_name%TYPE;
BEGIN
select s_name into v_name from student where s_id=1;
DBMS_OUTPUT.PUT_LINE('STUDENT NAME' ||' '||v_name);
END;
/
--constants
DECLARE
const CONSTANT NUMBER(7,6) NOT NULL DEFAULT 3.1415926;
BEGIN
DBMS_OUTPUT.PUT_LINE(const);
END;
/
--bind variables
--declaring
VARIABLE v_bind1 VARCHAR2(30);
VARIABLE v_bind2 VARCHAR2(30);
VARIABLE;
VARIABLE v_bind1;
--initialising
EXEC :v_bind1 :='sathya';
SET SERVEROUTPUT ON;
BEGIN
:v_bind2 :='dharma';
DBMS_OUTPUT.PUT_LINE(:v_bind2);
END;
/
print v_bind1;
print :v_bind1;
print;
--USING AUTOPRINT
SET AUTOPRINT ON;
VARIABLE v_bind3 VARCHAR2(30);
EXEC :v_bind3 :='love';
/
--conditional control statements
--1.CASE STATEMENTS
--2.IF STATEMENTS
--i.IF THEN
DECLARE
f_name VARCHAR2(30) :='sai';
l_name VARCHAR2(30) :='ganesh';
BEGIN
IF f_name='sai' and l_name='ganesh'
THEN
DBMS_OUTPUT.PUT_LINE('FULL NAME' ||' '||f_name||l_name);
END IF;
DBMS_OUTPUT.PUT_LINE('THANK YOU');
END;
/
--IF THEN ELSE
SET SERVEROUTPUT ON;
DECLARE 
v_num number :=&enter_a_number;
BEGIN
IF MOD(v_num,2)=0 THEN
DBMS_OUTPUT.PUT_LINE(v_num ||' '||'is even');
ELSE
DBMS_OUTPUT.PUT_LINE(v_num||' '||'is odd');
END IF;
DBMS_OUTPUT.PUT_LINE('DONE');
END;
/
--IF THEN ELSIF
SET SERVEROUTPUT ON;
DECLARE 
v_game varchar2(30) :='&entergame';
BEGIN
IF v_game ='cricket' THEN 
DBMS_OUTPUT.PUT_LINE('MSD');
ELSIF v_game='badminton' THEN
DBMS_OUTPUT.PUT_LINE('LEE CHONG WEI');
ELSIF v_game='basketball' THEN
DBMS_OUTPUT.PUT_LINE('JORDAN');
END IF;
DBMS_OUTPUT.PUT_LINE('THANK YOU');
END;
/
--LOOPS(iterative statements)
/*
1.SIMPLE LOOP
2.WHILE LOOP
3.NUMERIC FOR LOOP
4.CURSOR FOR LOOP
*/
--1.SIMPLE LOOP
SET SERVEROUTPUT ON;
DECLARE
v_counter number :=&enter_a_number;
v_result number;
BEGIN
LOOP
--v_counter:=v_counter+1;
v_result:=19*v_counter;
DBMS_OUTPUT.PUT_LINE('19'||'x'||v_counter||'='||v_result);
v_counter:=v_counter+1;
EXIT WHEN v_counter>=10;
END LOOP;
END;
/
--2.WHILE LOOP
SET SERVEROUTPUT ON;
DECLARE
v_counter number:=&enter_a_number;
v_result number;
BEGIN
WHILE v_counter<=10 
LOOP
v_result:=15*v_counter;
DBMS_OUTPUT.PUT_LINE('15x'||v_counter||'='||v_result);
v_counter:=v_counter+1;
END LOOP;
END;
/
--3.FOR LOOP
/*
FOR loop_counter IN [REVERSE] lower limit.. upper_limit LOOP
  Statement 1;
  Statement 2;
  …
  Statement 3;
END LOOP;
*/
SET SERVEROUTPUT ON;
BEGIN
  FOR v_counter IN 1..10 LOOP
    DBMS_OUTPUT.PUT_LINE(v_counter);
  END LOOP;
END;
/
BEGIN
  FOR v_counter IN REVERSE 1..10 LOOP
    DBMS_OUTPUT.PUT_LINE(v_counter);
  END LOOP;
END;
/
DECLARE
v_result number;
BEGIN
FOR v_counter IN 1..10 LOOP
v_result :=19*v_counter;
DBMS_OUTPUT.PUT_LINE(v_result);
END LOOP;
END;
/
--TRIGGERS
CREATE TABLE TABLE1 (
id number ,
name varchar2(30),
place varchar2(30)
);
INSERT INTO table1 ValUES (1,'sai','parthi');
INSERT INTO table1 ValUES (2,'lokesh','brahmanapalli');
INSERT INTO table1 ValUES (3,'akhil','kadapa');
INSERT INTO table1 ValUES (4,'zahan','dubai');
SELECT * FROM table1;
--DROP TABLE table1;
--1.DML TRIGGERS
SET SERVEROUTPUT ON;
CREATE OR REPLACE TRIGGER t_igger
BEFORE INSERT ON table1
FOR EACH ROW
ENABLE
DECLARE
v_user varchar2(30);
BEGIN
SELECT user INTO v_user FROM dual ;
DBMS_OUTPUT.PUT_LINE('You JUST inserted a row MR.'||v_user);
END;
/
--INSERT INTO table1 VALUES('5','RAJESH','SH');
CREATE OR REPLACE TRIGGER t_igger
BEFORE UPDATE ON table1
FOR EACH ROW
ENABLE
DECLARE
v_user varchar2(30);
BEGIN
SELECT user INTO v_user FROM dual;
DBMS_OUTPUT.PUT_LINE('you just updated a row Mr.'||v_user);
END;
/
--UPDATE table1 SET place='SuperHospital' where id=5;
CREATE OR REPLACE  TRIGGER t_igger
BEFORE INSERT OR DELETE OR UPDATE ON table1
FOR EACH ROW
ENABLE
DECLARE
v_user varchar2(30);
BEGIN
SELECT user INTO v_user FROM dual;
IF INSERTING THEN
DBMS_OUTPUT.PUT_LINE('you just updated a row Mr.'||v_user);
ELSIF DELETING THEN 
DBMS_OUTPUT.PUT_LINE('you deleted a row Mr.'||v_user);
ELSIF UPDATING THEN
DBMS_OUTPUT.PUT_LINE('you just updated a row Mr.'||v_user);
END IF;
END;
/
--UPDATE table1 SET place='Superhospital' where id=5;

CREATE TABLE t_audit (
new_name varchar2(30),
old_name varchar2(30),
user_name varchar2(30),
entry_date varchar2(30),
operation varchar2(30)
);
select * from t_audit;
--DROP TABLE t_audit;
CREATE OR REPLACE TRIGGER t_igger
BEFORE INSERT OR DELETE OR UPDATE ON table1
FOR EACH ROW
ENABLE
DECLARE
v_user varchar2(30);
v_date varchar2(30);
BEGIN 
SELECT user ,TO_CHAR(sysdate,'DD/MM/YYYY HH24:MI:SS')INTO v_user ,v_date FROM dual;
IF INSERTING THEN
INSERT INTO t_audit VALUES(:new.name,null,v_user,v_date,'INSERT');
ELSIF DELETING THEN
INSERT INTO t_audit VALUES(null,:old.name,v_user,v_date,'DELETE');
ELSIF UPDATING THEN
INSERT INTO t_audit VALUES(:new.name,:old.name,v_user,v_date,'UPDATE');
END IF;
END;
/
--UPDATE table1 SET place='superhospital' where id=5;
--INSERT INTO table1 VALUES('6','rahul','bellandur');
--DELETE FROM table1 WHERE ID=5;
--Synchronized Table Backup
CREATE TABLE table1_backup AS SELECT * FROM table1 ;
--DROP TABLE table1_backup;
SELECT * FROM table1;
SELECT * FROM table1_backup;
CREATE OR REPLACE TRIGGER t_igger
BEFORE INSERT OR DELETE OR UPDATE ON table1
FOR EACH ROW
ENABLE
BEGIN
IF INSERTING THEN
    INSERT INTO table1_backup  VALUES (:new.id,:NEW.NAME,:new.place);  
  ELSIF DELETING THEN
    DELETE FROM table1_backup WHERE NAME =:old.name; 
  ELSIF UPDATING THEN
    UPDATE table1_backup 
    SET NAME =:new.name WHERE NAME =:old.name;
  END IF;
END;
/
--INSERT INTO table1 VALUES('7','shanka','bellandur');