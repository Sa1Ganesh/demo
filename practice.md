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