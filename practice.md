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
