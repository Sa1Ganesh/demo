Methods(functions)

Collection methods are PL/SQL’s in-built functions and procedures which can be used in conjunction with collections.

Using PL/SQL collection method you can get the information as well as alter the content of the collection.

In Oracle Database we have 3 Collection Procedures and 7 Collection functions

Collection Functions

Count
Exists
First, Last
Limit
Prior, Next

Collection Procedures

Delete
Extend
Trim

Since the syntax for using the collection built-ins is different from the normal syntax used to call procedures and functions therefore they are referred to as methods.

In Oracle PL/SQL, collection methods can be used using Dot (.) notation.

the syntax.
Collection. Method (parameters) 

1.
Collection method COUNT ( ) returns the number of elements in an initialize collection. If used with an initialize collection with no elements; it returns zero.

Collection method COUNT ( ) returns zero when applied or used with an initialize collection (i.e. VARRAYs & Nested Tables) with no elements. It also returns zero  when it is used with an empty associated array.

The signature of the function COUNT is –

FUNCTION COUNT RETURN PLS_INTEGER;

Whenever you apply collection function COUNT ( ) to an uninitialized collection (i.e. Nested Tables & VARRAYs) it raises the ‘Collection_Is_Null’ exception which is a pre-defined exception in Oracle Database.
As Associative Arrays don’t require initialization, thus you will not get this exception with them

ex:
set serveroutput on;
declare 
type coun is table of number ;
var_c coun := coun (1,5,10,20,25,30,35,40);
begin 
dbms_output.put_line(var_c.count);
end;
/
set serveroutput on;
declare 
type coun is table of number ;
var_c coun := coun (1,5,10,20,25,30,35,40);
begin 
if var_c.count >=8 then
dbms_output.put_line('true');
end if;
dbms_output.put_line('yes');
end;
/
set serveroutput on;
declare 
type coun is table of number ;
var_c coun := coun (1,5,10,20,25,30,35,40);
begin 
for i in 1..var_c.count
loop
dbms_output.put_line('value at ' || ' ' || i || ' ' ||var_c(i));
end loop;
end;

2.
Collection Method EXISTS ( ) checks the existence of an element at a specific index in a collection.  If it finds the specified element then it returns TRUE otherwise it returns FALSE.

The syntax of EXISTS ( ) function is –

EXISTS (index number);

EXISTS function takes the subscript/index number of a cell of the collection as an Input and searches it in the collection. If it finds any element corresponding to index number then it returns TRUE otherwise it returns FALSE.

EXISTS function does not return null. It either returns True or False.

If you remove a row using Trim or Delete function then collection method EXISTS ( ) will return FALSE for the index of that row.

collection method EXISTS does not raise any exception. In fact, this is the only function which does not raise any exception, even if it used with an uninitialized collection.

Collection method EXISTS ( ) returns false, if it is applied either to an uninitialized collection or to an initialize collection with no elements.

set serveroutput on;
declare 
type exi is table of varchar2(30);
var_e exi := exi ('sathya','dharma','shanti','prema');
begin 
if var_e.exists(5) then
dbms_output.put_line('value at 1' || ' ' ||var_e(1));
else
dbms_output.put_line('no value');
end if;
end;
/
set serveroutput on;
declare 
type exi is table of varchar2(30);
var_e exi := exi ('sathya','dharma','shanti','prema');
begin 
if var_e.exists(5) then
dbms_output.put_line('value at 5' || ' ' ||var_e(5));
else
dbms_output.put_line('no value');
var_e.extend;
var_e(5):='love';
end if;
dbms_output.put_line(' value is '||' '||var_e(5));
end;

3,4.Collection methods FIRST ( ) and LAST ( )

We use collection functions First & Last to know the first and last index values defined in a collection

You can use both these functions with all three types of collections that are Associative Array, Nested table and VARRAYs.

these functions return the index number of the collection.

Both the functions return null when applied to an empty collection or when applied to an initialize collection that has no elements.

 The specification for collection function FIRST ( ) is:

FUNCTION FIRST RETURN PLS_INTEGER | VARCHAR2

And the function specification for collection function LAST ( ) is:

FUNCTION LAST RETURN PLS_INTEGER | VARCHAR2

For string indexed associative array, these methods return strings; “lowest” and “highest” are determined by the ordering of the character set in use in that session

if there is only 1 element in my VARRAY?
In that case collection function FIRST ( ) is always 1 and collection method LAST ( ) is always equal to COUNT.

 if you applied collection function FIRST & LAST to an uninitialized collection then it will raise COLLECTION_IS_NULL exception.

 If you delete the first element of the collection function then the collection function FIRST will return the subscript which is greater than 1 and is holding some data.

If you delete the data from the middle then the collection function LAST will return a value which is greater than the value returned by COUNT method.

ex:
set serveroutput on;
declare 
type coun is table of number ;
var_fl coun := coun (100,5,10,20,25,30,35,40);
begin 
--var_fl.delete(8);
--var_fl.trim;
--var_fl.delete(3,4);
dbms_output.put_line(var_fl.first);
dbms_output.put_line(var_fl(var_fl.first));
dbms_output.put_line(var_fl.last);
dbms_output.put_line(var_fl(var_fl.last));
end;
/

5.
Collection method LIMIT returns the maximum number of elements that a VARRAY can hold.by using this function you can find out how many elements you can store in a VARRAY.

Collection method LIMIT only works with VARRAY. If it applied to Nested table or associative array then this function will either return a NULL or No value

The specification of collection function LIMIT is:

FUNCTION LIMIT RETURN pls_integer;

the function LIMIT raises COLLECTION_IS_NULL exception if it is applied to an uninitialized nested table or a VARRAY.

The collection function LIMIT returns the total number of indexes of a VARRAY regardless of whether these indexes are empty or holding some data.  It checks the definition of the VARRAY and sees the total number of elements it is designed to store and returns that number.

While the collection function COUNT returns the number of Indexes which are not empty and holding some data.

ex:
set serveroutput on;
declare 
type lim is varray(5) of number ;
var_l lim := lim (1,2,3);
begin 
--var_l.extend;
--var_l(1):=5;
dbms_output.put_line(var_l.limit);
dbms_output.put_line(var_l.count);
end;
/

6,7.Prior and Next Collection Functions

Both these functions take an index of the collection as input and return the result.

PL/SQL collection method PRIOR takes an index as input and returns the value stored into the previous lowest index. Whereas the collection method NEXT returns the value from the next higher index.

Both Prior and Next collection functions can be used with all the three types of collections. 

both these collection functions return Null if they are used with the First and Last indexes of a collection respectively.

If either of these functions are applied to an uninitialized Nested Table or a Varray then they raise the COLLECTION_IS_NULL exception.

ex:
set serveroutput on;
declare 
type prion is table of number ;
var_pn prion := prion (100,5,10,20,25,30,35,40);
begin 
dbms_output.put_line(var_pn.prior(3));
dbms_output.put_line(var_pn(var_pn.prior(3)));
dbms_output.put_line(var_pn.next(3));
dbms_output.put_line(var_pn(var_pn.next(3)));
end;
/

PROCEDURES ()Methods

1.PL/SQL collection method Delete?

Collection method DELETE is an overloaded procedure which removes elements from the collection.

you can use the same procedure in three different ways. 

i.DELETE: Simple procedure call without any parameters. if used without any parameter then it will remove all the elements from the collection.

ii.DELETE (index-number): with a single parameter,it is the valid index number of the collection. Collection procedure DELETE called by passing a valid index number will remove the element of the specific index.

iii.DELETE (start-index, ending-index): Procedure call with two parameters. is termed as Range delete. you have to specify two Indexes.the procedure deletes the range of elements which fall between starting-index and ending-index.

collection method DELETE can be used will all three types of collections

As VARRAY is not a sparse collection hence we cannot delete individual rows from it.only procedure call that we can execute with VARRAY is the collection method DELETE without any arguments which removes all . 

If the procedure DELETE is applied to an uninitialized Nested Table and VARRAY then it raises a “Collection_is_Null” exception.  

ex:
i.Simple procedure call without argument.

2.Procedure call with a single parameter
set serveroutput on;
declare 
type del_c is table of number ;
var_dl del_c := del_c (0,5,10,20,25,30,35);
begin 
var_dl.delete(5);
for i in 1..var_dl.last 
loop
if var_dl.exists(i) then
dbms_output.put_line('value at '||i||' '||var_dl(i));
/*else
dbms_output.put_line('no value');*/
end if;
end loop;
end;
/
set serveroutput on;
declare 
type del_c is table of number ;
var_dl del_c := del_c (0,5,10,20,25,30,35);
begin 
var_dl.delete(5);
if var_dl.exists(5) then
dbms_output.put_line('value at'||var_dl(5));
else 
dbms_output.put_line('no value' );
end if;
end;

3.Procedure call with two parameters.
set serveroutput on;
declare 
type del_c is table of number ;
var_dl del_c := del_c (0,5,10,20,25,30,35);
begin 
var_dl.delete(2,5);
for i in 1..var_dl.last 
loop
if var_dl.exists(i) then
dbms_output.put_line('value at '||i||' '||var_dl(i));
/*else
dbms_output.put_line('no value');*/
end if;
end loop;
end;

2. PL/SQL Collection Method EXTEND
Collection method EXTEND is an overloaded PL/SQL procedure which is used for appending elements to the collection.
 we can call this same procedure in different ways.

i.Extend: Extend procedure call without any argument.
Calling PL/SQL Collection procedure Extend without any argument will append a single NULL element to the collection.

ii.Extend (n): Extend procedure call with one argument.
Collection procedure Extend with one argument will append the number of NULL elements that you mentioned as the argument of the procedure. But, remember that the argument must be a valid Integer value.

iii.Extend (n, v): Extend procedure call with two arguments.
In this case the first argument indicates the number of elements that will be appended to the collection. Moreover the second argument is the index number. Moreover it’s value will be copied and assigned to each of the newly appended elements of the collection. This form of EXTEND is required for collections with “not null elements”.

Collection Method EXTEND can only be applied to collection Nested Tables and VARRAYs. EXTEND cannot be used with collection Associative Arrays.

the overloaded specifications of PL/SQL Collection Method EXTEND

EXTEND Procedure with one argument:
PROCEDURE EXTEND (n pls_integer := 1);

Collection Method EXTEND with two arguments:
PROCEDURE EXTEND (n pls_integer, v pls_integer);

When you have a collection (either Nested Table or VARRAY) in your code which is not initialized with sufficient number of elements. In that case you must first use PL/SQL Collection Method EXTEND.

before storing the data into the index we need to create a memory slot for it. Consequently, PL/SQL Collection procedure EXTEND helps us in creating that memory slot for that data.  

If PL/SQL Collection Method EXTEND is applied to an uninitialized collection then it will show a COLLECTION_IS_NULL exception.

If collection method EXTEND is used with VARRAY to extend it beyond its defined limit then you will have to face another exception which is SUBSCRIPT_BEYOND_LIMIT.

ex:
i.
PL/SQL Collection Procedure EXTEND with No Argument.
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

ii.
Collection Procedure EXTEND with One Argument.
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
iii. SET SERVEROUTPUT ON;
DECLARE
    TYPE my_nestedTable IS TABLE OF number;
    nt_obj  my_nestedTable := my_nestedTable();
BEGIN
    nt_obj.EXTEND;
    nt_obj(1) := 10;
    nt_obj.EXTEND(6,1);
    DBMS_OUTPUT.PUT_LINE ('Data at index 1 is '||nt_obj(1));
    DBMS_OUTPUT.PUT_LINE ('Data at index 2 is '||nt_obj(2));
    DBMS_OUTPUT.PUT_LINE ('Data at index 3 is '||nt_obj(3));
    DBMS_OUTPUT.PUT_LINE ('Data at index 4 is '||nt_obj(4));
    DBMS_OUTPUT.PUT_LINE ('Data at index 5 is '||nt_obj(5));
    DBMS_OUTPUT.PUT_LINE ('Data at index 6 is '||nt_obj(6));
END;
/
 Collection Procedure EXTEND (No Argument) with VARRAY
SET SERVEROUTPUT ON;
DECLARE
    TYPE my_Varray IS VARRAY (5) OF NUMBER;
    vry_obj my_Varray := my_Varray();
BEGIN
    vry_obj.EXTEND;
    vry_obj(1) := 10;
    DBMS_OUTPUT.PUT_LINE('Data at index 1 is '||vry_obj(1));
END;
/

3.PL/SQL Collection Method TRIM
Collection Method TRIM in Oracle db is an overloaded procedure using which you can remove one or more elements from the end of a collection.
we can call it in two different ways. 

TRIM: Trim procedure without parameter.
If we use TRIM procedure without any parameters then it will remove only one element from the end of the collection.

TRIM: Trim procedure with one parameter.
TRIM procedure called by passing one argument will remove number of elements from the end of the collection as specified by the parameter. That parameter must be a valid Integer number. Also it should not be greater than the maximum number of elements that your collection has.

procedure TRIM can only be used with collection Nested Tables and VARRAYs. However we cannot use it with Associative Arrays.

The specification of collection procedure TRIM is:

PROCEDURE TRIM (n PLS_INTEGER:= 1);

PL/SQL Collection Method TRIM will raise SUBSCRIPT_BEYOND_COUNT exception. If there is an attempt to trim more elements than actually exists in the collection.

we cannot use collections TRIM and DELETE procedure together. Using them with each other will produce unexpected results.

Think about a scenario when you have DELETED an element situated at the end of a VARRAY variable and thereafter apply TRIM procedure on the same. Can you guess the number of elements that you may have removed? You might be confused into believing that two elements have been removed however the fact is that only one has been removed. TRIM acts upon the placeholder left by procedure DELETE.


In order to steer clear of these confusing and unnerving results, Oracle database support highly recommends that these two procedures should be used exclusively on a given collection.

ex:
1. PL/SQL Collection Procedure TRIM without parameter.
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

2. Collection Procedure TRIM with parameter.
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

3. PL/SQL Collection Procedure TRIM SUBSCRIPT_BEYOND_COUNT exception.
SET SERVEROUTPUT ON;
DECLARE
    TYPE my_nestedTable IS TABLE OF number;
    nt_obj  my_nestedTable := my_nestedTable(1,2,3,4,5);
BEGIN
    nt_obj.TRIM(6);
    DBMS_OUTPUT.PUT_LINE ('After TRIM procedure');
    FOR i IN 1..nt_obj.COUNT
    LOOP
        DBMS_OUTPUT.PUT_LINE (nt_obj(i));
    END LOOP;
END;
/
SUBSCRIPT_BEYOND_COUNT exception occurs when we pass an argument greater than the total number of elements of the collection

4. Collection Procedure TRIM with VARRAY.
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

PL/SQL Ref Cursors In Oracle Database
Ref Cursor is an acronym of Reference to a Cursor. It is a PL/SQL datatype using which you can declare a special type of variable called Cursor Variable.

cursor variable is like a pointer which refers to different context area in SGA. Whereas Ref Cursor is a datatype which holds a cursor value.

A variable of Ref Cursor Type is called Cursor Variable.

Cursor Variables are declared with the help of Ref Cursors. Consequently, we can say that in Oracle Database cursor variables exist in the form of Ref Cursors.

syntax for declaring Ref cursors in Oracle Database:

DECLARE
 TYPE [cursor_variable_name] IS REF CURSOR [RETURN (return_type)];

There are two types of Ref Cursors in PL/SQL. These are:

1.Strong Ref Cursor 
Any Ref Cursor which has a fixed return type is called a Strong Ref Cursor.

Moreover such ref cursors can only be used with some SELECT statements. In addition, the result of SELECT statement’s datatype should match with the one that was fixed during the strong cursor’s declaration.

 syntax.

DECLARE
	TYPE cursor_variable_name IS REF CURSOR 
RETURN (return type);

Return clause plays a very vital role in declaring a Ref Cursor. It restricts its scope. And makes your Ref Cursor limited to only those SELECT statements that return the result whose datatype matches with the one you specified in the RETURN clause while declaring it.

Also the return type of a Ref Cursor must always be of Record Datatype. It can either be record structure of a table or user defined record structure.

2.Weak Ref Cursor
weak ref cursors are those which do not have any return type.

Since weak Ref Cursors do not have any fixed return type thus they are open to all SELECT statements. And this makes them one of the most used Ref Cursors in PL/SQL.

Syntax of Weak Ref Cursors in PL/SQL
DECLARE
	TYPE ref_cursor_name IS REF CURSOR;

Sys_RefCursor

Sys Ref cursor is an Oracle built in cursor variable. It declares a weak ref cursor and that too without declaring the ref pointer type. Mostly it is used as a generic cursor which can be passed as an argument to a stored sub program.

1.PL/SQL Strong Ref Cursors in Oracle Database
A ref cursor which has fixed return type is called a Strong Ref Cursor in Oracle Database. Because of fixed return type, strong ref cursors can only be used selectively. For instance with those SELECT statements that return the result whose datatype matches with the one that you have fixed during cursor’s declaration.

The return type of a strong ref cursor must always be a Record Datatype.  It can either be a Table Based Record datatype or a User Defined Record Datatype.

Create a ref pointer type.

TYPE	my_RefCur	IS REF CURSOR RETURN 	employees%ROWTYPE;

you first write the keyword TYPE followed by the name of your ref cursor. Thereafter you have to write a reserved phrase IS REF CURSOR. It will tell the compiler that we are creating a type which is REF CURSOR. Followed by that you have to specify the RETURN clause.

Create a Cursor Variable

cur_var my_RefCur;

to create a cursor variable you have to first write the name of your variable followed by the name of your Ref Cursor. Thereafter this variable will be used to refer to the Ref Cursor over which it is created.

ex:
Strong Ref Cursor With table based Record Datatype
SET SERVEROUTPUT ON
 DECLARE
    	/*Create Ref Pointer Type*/
	TYPE	my_RefCur	IS REF CURSOR RETURN 	employees%ROWTYPE;
	/*Create Cursor Variable*/
	cur_var my_RefCur;
	rec_var     employees%ROWTYPE;
 BEGIN
	OPEN cur_var FOR SELECT * FROM employees WHERE employee_id = 100;
	FETCH cur_var INTO rec_var;
	CLOSE cur_var;
	DBMS_OUTPUT.PUT_LINE ('Employee '||rec_var.first_name||' has salary '||rec_var.salary||'.');
END;
/

Strong Ref Cursor With User Defined Record Datatype

SET SERVEROUTPUT ON;
DECLARE
	--Create User-Defined Record Datatype
    TYPE my_rec IS RECORD (
        emp_sal employees.salary%TYPE
        );
	--Declare Strong Ref Cursor
    TYPE RefCur IS REF CURSOR RETURN my_rec;
    cur_var REFCUR;
	--Another anchored datatype variable for holding data
    at_var  employees.salary%TYPE;
BEGIN
   OPEN cur_var FOR SELECT salary FROM employees WHERE employee_id = 100;
    FETCH cur_var INTO at_var;
    CLOSE cur_var;
    DBMS_OUTPUT.PUT_LINE ('Salary of the employee is '||at_var);
END;
/

2.PL/SQL Weak Ref Cursor 
A ref cursor which does not have a fixed return type is called a Weak Ref Cursor.They are open to all types of SELECT statements.

 PL/SQL Weak Ref Cursor is the alternative way of fetching data of different datatypes. As weak ref cursor doesn’t have a fixed return type thus there is no need of creating a separate record datatype.

Syntax of Weak Ref Cursor
TYPE ref_cursor_name IS REF CURSOR;

SET SERVEROUTPUT ON;
DECLARE
    /*Declare Weak Ref Cursor*/
    TYPE wk_RefCur IS REF CURSOR;
    /*Declare Cursor Variable of ref cursor type*/
    cur_var wk_RefCur;
    
     /*Declare two "Anchored Datatype Variable" for holding data from the cursor*/
    f_name  employees.first_name%TYPE;
    emp_sal employees.salary%TYPE;
BEGIN
    OPEN cur_var FOR SELECT first_name, Salary FROM employees WHERE employee_id = 100;
    FETCH cur_var INTO f_name, emp_sal;
    CLOSE cur_var;
    DBMS_OUTPUT.PUT_LINE (f_name ||' '||emp_sal);
END;
/

3.PL/SQL SYS_REFCURSOR
SYS_REFCURSOR is a predefined weak ref cursor which comes built-in with the Oracle database software.

There is no space between REF and CURSOR. It is a single word “RefCursor”.
Correct: SYS_REFCURSOR

ex:
SET SERVEROUTPUT ON;
DECLARE
    --Declare cursor variable of SYS_REFCURSOR type
    cur_var SYS_REFCURSOR;
    
    --Declare variables for holding data
    f_name  employees.first_name%TYPE;
    emp_sal employees.salary%TYPE;
BEGIN
OPEN cur_var FOR SELECT first_name, Salary FROM employees WHERE employee_id = 100;
    FETCH cur_var INTO f_name, emp_sal;
    CLOSE cur_var;
    DBMS_OUTPUT.PUT_LINE(f_name ||' '||emp_sal);
END;
/

Bulk collect

Bulk collect is all about reducing context switches and improving the performance of the query

Context Switching

Whenever you write a PL/SQL block or say a PL/SQL program and execute it, the PL/SQL runtime engine starts processing it line by line. This engine processes all the PL/SQL statements by itself but it passes all the SQL statements which you have coded into that PL/SQL block to the SQL runtime engine. Those SQL statements will then get processed separately by the SQL engine. Once it is done processing them the SQL engine then returns the result back to the PL/SQL engine. So that a combined result can be produced by the latter. This to and fro hopping of control is called context switching.

Context switching has a direct impact on the performance of the query. The higher the hopping of controls the greater will be the overhead which in turn will degrade the performance. This means that lesser the context switching better will be the query performance.

Bulk collect clause compresses multiple switches into a single context switch and increases the efficiency and performance of a PL/SQL program.

Bulk collect clause reduces multiple control hopping by collecting all the SQL statement calls from PL/SQL program and sending them to SQL Engine in just one go and vice versa.

Bulk collect clause can be used with SELECT-INTO, FETCH-INTO and RETURN-INTO clauses.

With the help of Bulk Collect Statement we can SELECT, INSERT, UPDATE or DELETE large data sets from database objects such as Tables or Views.

The process of fetching batches of data from PL/SQL runtime engine to SQL engine and vice versa is called Bulk Data Processing.

We have one bulk data processing clause which is Bulk Collect and one bulk data processing statement which is FORALL in Oracle Database.

We can use Bulk collect clause either inside a SQL Statement or with FETCH statement. When we use bulk collect clause with SQL Statement i.e. SELECT INTO it uses implicit cursor. Whereas if we use bulk collect clause with FETCH statement it uses explicit cursor.

1.Bulk Collect Clause With SELECT INTO Statement

“Bulk Collect Into” statement selects multiple data from a column and stores it into a SQL Collection.

When you are certain that the returning result of your SELECT statement is small then you should use Bulk Collect clause with Select-Into statement. Otherwise your bulk collect clause will make your Select-Into statement a memory hogging monster. Consequently it will slowdown the performance of your database. 

You can always use LIMIT clause along with the Bulk Collect to limit the number of rows fetched from the database. But this is only possible when we are using the Bulk Collect clause with PL/SQL Cursors. 

When we use Bulk Collect clause with SELECT-INTO statement it uses implicit cursor to perform the task of bulk data processing.Limit Clause can only be used with Bulk Collect clause,On the other hand whenever we use Bulk Collect clause with the FETCH statement it uses an explicit cursor.

PL/SQL Collections are the only supportive datatypes for Bulk Data Processing with Bulk Collect Clause in Oracle Database. In case if you try to store the data retrieved using Bulk Collect clause into a variable of datatype such as Char, Number or Varchar2 you will get an error

Syntax of Bulk Collect clause with Select-Into statement.

SELECT column_list
 BULK COLLECT INTO collection_datatype_name 
FROM table_name 
WHERE <where clause> 
ORDER BY <column list>;

always remember every column you specified for retrieving the data must carry a corresponding collection datatype for holding that data. Because PL/SQL runtime engine always stores the data retrieved from the column into the collection in parallel manner.

SELECT column_1, column_2 BULK COLLECT INTO collection_1, collection2 FROM table;

ex:
SET SERVEROUTPUT ON;
DECLARE
    TYPE nt_fName   IS TABLE OF VARCHAR2 (20);
    TYPE nt_lName   IS TABLE OF VARCHAR2 (20);
    
    fname   nt_fName;
    p_lace   nt_lName;
BEGIN
    SELECT name, place 
        BULK COLLECT INTO fname, p_lace 
    FROM table1; 
        
        --Print values from the collection--
    FOR i IN 1..fname.COUNT
    LOOP
        DBMS_OUTPUT.PUT_LINE (i||' - '||fname(i) ||' '||p_lace(i));
    END LOOP;
END;
/

2.PL/SQL Bulk Collect Clause With FETCH INTO Statement

SELECT-INTO statement is a SQL standard query which means developer does not have much control over the execution of the statement.

If we talk about query performance, we cannot go beyond an extent with SELECT-INTO statement

if we use Bulk Collect with FETCH-INTO statement then the runtime engine will use the explicit cursor to process the task.

An explicit cursor always helps us in getting advance control over our standard SQL queries. For example with an explicit cursor we can control when to fetch the records or how many records we want to retrieve at once however this is not possible in case of SELECT-INTO statement.

the syntax

FETCH <cursor_name> BULK COLLECT INTO <plsql_collection>;

FETCH statements are part of explicit cursor. If you try to execute them without declaring their parent cursor then you will get an error. Also always remember that PL/SQL collections are the only supported structure for bulk collect.

ex:
SET SERVEROUTPUT ON;
DECLARE
--Create an explicit cursor
    CURSOR exp_cur IS
    SELECT name FROM table1;

    --Declare collection for holding the data 
    TYPE nt_fName   IS TABLE OF VARCHAR2 (20);
    fname   nt_fName;
BEGIN
    OPEN exp_cur;
    LOOP
        FETCH exp_cur BULK COLLECT INTO fname;
        EXIT WHEN fname.count=0;
        --Print data
         FOR idx IN fname.FIRST.. fname.LAST
        LOOP
            DBMS_OUTPUT.PUT_LINE (idx||' '||fname(idx) );
        END LOOP; 
    END LOOP;
    CLOSE exp_cur;
END;
/

Bulk Collect With LIMIT Clause In Oracle Database

there is still a problem which requires our attention and that is excessive memory exhaustion caused by bulk collect.

Whenever we retrieve or fetch a large number of records using bulk collect clause, our program starts consuming a lot of memory in order to be fast and efficient. This is not just any memory. Unlike the SGA memory that is shared among all the sessions of Oracle Database, the program consumes the PGA memory that is specifically allotted for each session.


This degrades the performance of the database. This means that our query must surely be performing well but at the same time, our database may not.


We cannot have a well-optimized query by compromising the performance of our entire database.

This problem of memory exhaustion can easily be overcome  if we can control and constraint the amount of data fetched using the bulk collect. We can do that by using Bulk Collect with the LIMIT clause.

LIMIT clause works as an attribute of a FETCH-INTO statement:

FETCH <cursor_name> BULK COLLECT INTO <plsql_collection> LIMIT number;

LIMIT clause restricts the number of rows fetched using BULK COLLECT with FETCH statement

The LIMIT clause can only be used when you are using BULK COLLECT with FETCH-INTO statement. It cannot be used when you are using bulk collect with SELECT-INTO statement.

ex:
SET SERVEROUTPUT ON;
DECLARE
--Create an explicit cursor
    CURSOR exp_cur IS
    SELECT name FROM table1;

    --Declare collection for holding the data 
    TYPE nt_fName   IS TABLE OF VARCHAR2 (20);
    fname   nt_fName;
BEGIN
    OPEN exp_cur;
        FETCH exp_cur BULK COLLECT INTO fname limit 2;
         CLOSE exp_cur;
         FOR idx IN 1..fname.count
        LOOP
            DBMS_OUTPUT.PUT_LINE (idx||' '||fname(idx) );
      END LOOP;
END;
/

FORALL

Bulk Data Processing Using FORALL Statement

FORALL statement helps to process bulk data in an optimized manner by sending DML statements or a MERGE statement in batches from PL/SQL engine to SQL engine.

Bulk collect optimizes the query and boosts the performance by reducing the context switches. FORALL does the same thing for DML statements like Insert, Delete, Update or for a MERGE statement.


the syntax of FORALL statement

FORALL index IN bound_clauses 
[SAVE EXCEPTION]
DML statement;

Index is an implicitly defined loop counter which is declared by PL/SQL engine as PLS_INTEGER. As it is implicitly defined by PL/SQL engine thus you do not need to define it. The scope of Index is limited to the FORALL statement in which it is defined.

Bound_Clauses are the clauses which control the number of the loop iterations. Also the value of index is also dependent on it. There are three types of bound clauses in Oracle PL/SQL

SAVE EXCEPTION is an optional choice which keeps the FORALL statement running even when DML statement causes an exception. These exceptions are saved in a cursor attribute called SQL%Bulk_Exceptions.

Because of SAVE EXCEPTION, FORALL statement does not exit abruptly even when there is an exception. This is an advantage of FORALL statement over FOR Loop.

you need to make sure that your DML statement or the MERGE statement must be referencing at least one collection in its VALUES or WHERE clause.

with FORALL statement we can only use one DML at a time. This is the shortcoming of FORALL. you can either execute Insert or Update at a time not both together.

the cursor attribute SQL%Bulk_Exceptions is a collection of records which has two fields Error_Index and Error_Code. Error_Index stores the number of iterations of the FORALL statement in the course of which the exception has occurred. On the other hand Error_Code stores the exception code that is corresponding to the raised exception.

You can easily check how many exceptions has been raised during the execution of the FORALL statement using SQL%BULK_EXCEPTION.COUNT.

Bound clauses control the value of loop index and the number of iterations of the FORALL statement. There are three types of bound clauses which can be used with the FORALL statement in Oracle Database.

1.Lower & Upper Bound
2.Indices of and
3.Values of

The LOWER AND UPPER bound: Similar to FOR LOOP, in this bound clause you have to specify the valid starting and the ending of the consecutive index numbers of the referenced collection.

It is a must to specify a valid range of consecutive index numbers along with this Bound Clause for the number of collection(s) referenced in the DML statement. However there stands a chance of error if the collection that is referenced with this clause is found to be sparse. The error that you will get is this:

ORA-22160: element at index [3] does not exist
In case your referenced collection is sparse and you are using Oracle 10g or above then you might want to use the other two options which are, ‘Indices-of’ and ‘Values-of’.  

The INDICES OF:  The second bound clause which is available to us is ‘Indices of’. This bound clause enables our FORALL statement to loop through a sparse collection such as an associative array or a nested table.

The VALUES OF: The third bound clause is Values of. The VALUES OF option makes it clear that the values of the elements of the specified collection of the loop counter is the basis of the values in FORALL statement. Basically, this collection is a group of indexes that the FORALL statement can loop through. Furthermore, these indexes need not be unique as well as can be listed in an arbitrary order.

1.FORALL Statement With Lower & Upper Bound Clause

With Lower & Upper Bound clause we have to specify the valid range of consecutive index numbers of the collection.

Lower & Upper Bound clause can only be used when the collection which you are referencing in your DML statement is Dense.

 FORALL statement does the same work as the bulk collect clause but in an inverse manner. For example, with bulk collect we were fetching the data from the tables and storing it into the collection, but now with FORALL statement we will fetch the data from the collection and store it into the table.

1.Create a Table.
First we will create a table. We will use this table to dump the data which we will be fetching from the collection.

2.Create & Populate the Collection.
 If you don’t have a collection then create and populate a collection.

3.Write the FORALL statement
Once you have your table and collection ready then write a FORALL statement which will fetch the data from the collection and store it into the table. 

ex:
SET SERVEROUTPUT ON;
CREATE TABLE tut_77 (
    Mul_tab    NUMBER(5)
);
That is going to be our table which will hold the data. Next we will write the PL/SQL block.

DECLARE
	-- Declare the collection
    TYPE My_Array IS TABLE OF NUMBER INDEX BY PLS_INTEGER;
    col_var My_Array;
	--Declare a variable for holding the total number of records of the table
    tot_rec NUMBER;
BEGIN
    --Populate the collection
    FOR i IN 1..10 LOOP
        col_var (i) := 9*i;
    END LOOP;
    -- Write the FORALL statement.
    FORALL idx IN 1..10
        INSERT INTO tut_77 (mul_tab)
        VALUES (col_var (idx));
    --Get the total number of records from the table     
    SELECT count (*) INTO tot_rec FROM tut_77;
    DBMS_OUTPUT.PUT_LINE ('Total records inserted are '||tot_rec);
END;
/

2. FORALL statement with INDICES OF clause 

 Lower & Upper bound clause of FORALL statement is that it cannot be used with a sparse collection. This shortcoming can easily be overcome by using options like ‘Indices-of’ or ‘Values-of’ bound clause. 

Similar to Lower & Upper bound clause INDICES-OF clause helps us in bulk data processing by letting us iterate through the collection. The only difference is that using INDICES OF clause we can iterate through a dense as well as a sparse collection. Whereas Lower & Upper bound clause works only with a dense collection.

Syntax of INDICES OF bound clause
FORALL index IN INDICES OF collection_variable
[SAVE EXCEPTION]
DML statements; 

after writing the reserved phrase ‘INDICES OF’ we have to specify the collection variable of the collection whose data we want to use.

FORALL statement takes the data from the collection and stores it into a table.

ex:
SET SERVEROUTPUT ON;
CREATE TABLE tut_78(
    mul_tab NUMBER(5)
);

DECLARE
    TYPE my_nested_table IS TABLE OF number;
    var_nt my_nested_table := my_nested_table (9,18,27,36,45,54,63,72,81,90);
    --Another variable for holding total number of record stored into the table 
    tot_rec NUMBER;
BEGIN
    var_nt.DELETE(3, 6);
    
    FORALL idx IN INDICES OF var_nt
        INSERT INTO tut_78 (mul_tab) VALUES (var_nt(idx));
        
    SELECT COUNT (*) INTO tot_rec FROM tut_78;
    DBMS_OUTPUT.PUT_LINE ('Total records inserted are '||tot_rec);
END;
/

using collection method DELETE I have deleted data from index 3 to index 6. This means that now the index of our collection is not populated sequentially. That changes the nature of our collection from DENSE to SPARSE.


3.FORALL Statement With VALUES-OF Bound Clause

FORALL statement is all about binding the collection elements with a single DML statement in an optimized manner. Using ‘Values-of’ bound clause of FORALL statement we can bind the selected elements of the collection with the DML statement.

FORALL idx IN VALUES OF indexing-collection
[Save exception]
DML/MERGE statement;

Values-of bound clause will require two collections. The first collection will be the ‘Source Collection’. We will be doing DML operations such as insert, delete & update on the data of this collection using the FORALL statement.

The second collection will be the ‘Indexing Collection’ which will specify the index number of selected elements from the first collection. These selected elements will be those elements over which you want to perform the DML operations.

Indexing collection is a group of indexes that the FORALL statement can loop through. This collection could be a dense collection as well as a sparse collection. Also the index numbers stored into the collection need not be unique and can be listed in an arbitrary order.  

Restrictions are –
i.The indexing collection must be a NESTED TABLE or an ASSOCIATIVE ARRAY.
ii.If the indexing collection is an associative array then it must be indexed by PLS_INTEGER or BINARY_INTEGER.
iii.The elements of the indexing collection must be of either PLS_INTEGER or BINARY_INTEGER.

ex:
CREATE TABLE tut_79 (
    selected_data     NUMBER(5)
);

SET SERVEROUTPUT ON;
DECLARE
    --Source collection
    TYPE My_NestedTable IS TABLE OF NUMBER;
    source_col My_NestedTable := My_NestedTable (9,18,27,36,45,54,63,72,81,90);
    
    --Indexing collection
    TYPE My_Array IS TABLE OF PLS_INTEGER INDEX BY PLS_INTEGER;
    index_col My_Array;
BEGIN
    --Initializing indexing collection with the index numbers.
    index_col   (1) :=  3;
    index_col   (5) :=  7;
    index_col   (12):=  8;
    index_col   (28):=  10;
    --FORALL statement 
    FORALL idx IN VALUES OF index_col
        INSERT INTO tut_79 VALUES (source_col (idx));
END;