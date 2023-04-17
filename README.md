# PL/SQL Cheat Sheet

## Blocks

PL/SQL uses a block structure to organize code and control the scope of variables and exceptions. The basic structure of a PL/SQL block consists of four parts: **DECLARE**, **BEGIN**, **EXCEPTION**, and **END**.
>SET SERVEROUTPUT ON is a SQL*Plus command that enables output to the console for DBMS_OUTPUT.PUT_LINE statements in PL/SQL.

```sql
SET SERVEROUTPUT ON;
DECLARE
 --Declaration statements

BEGIN
 --Executable statements

Exceptions
 --Exception handling statements

END;
```

## Variables

Data Types:
- Scalar
> Number, Date, Boolean, Character
- Large Object
> Large Text, Picture - BFILE, BLOB, CLOB, NCLOB
- Composite
> Collections, Records

```sql
v_number NUMBER(5,2) := 9.64;
v_character VARCHAR2(20) := 'test';
newyear DATE:='01-JAN-2020';
current_date DATE:=SYSDATE;
```

## Constant

```sql
DECLARE
	v_pi CONSTANT NUMBER(7,6) := 3.141592;
BEGIN
	DBMS_OUTPUT.PUT_LINE(v_pi);
END;
```

## Not Null

```sql
DECLARE
  v_name VARCHAR2(50) NOT NULL;
BEGIN
  v_name := 'John Smith';
  DBMS_OUTPUT.PUT_LINE('Name: ' || v_name);
END;
```
## Select Into

INTO is an SQL keyword used in SELECT statements to specify where to store the result of a query. It is used in combination with the SELECT keyword to retrieve data from a database table and store it in a variable or set of variables.

```sql
DECLARE
	v_last_name VARCHAR2(20);
BEGIN
	SELECT last_name INTO v_last_name FROM persons WHERE person_id = 1;
	DBMS_OUTPUT.PUT_LINE('Last name: ' || v_last_name);
END;
```

## %Type

In PL/SQL, %TYPE is a keyword that is used to declare a variable to be of the same data type as another variable, column, or table in the database.

```sql
DECLARE
	v_last_name persons.last_name%TYPE;
BEGIN
	SELECT last_name INTO v_last_name FROM persons WHERE person_id = 1;
	DBMS_OUTPUT.PUT_LINE('Last Name: ' || v_last_name);
END;
```

## Conditions

```sql
DECLARE
	v_num NUMBER := &enter_a_number;
BEGIN
	IF mod(v_num,2) = 0 THEN
	    dbms_output.put_line(v_num || ' is even');
	ELSIF mod(v_num,2) = 1 THEN
	    dbms_output.put_line(v_num || ' is odd');
	ELSE
	    dbms_output.put_line('None');
	END IF;
END;
```

## Loops

- ### Simple Loop

```sql
DECLARE
	v_num number(5) := 0;
BEGIN
	loop
	    v_num := v_num + 1;
	    dbms_output.put_line('Number: ' || v_num);
	    
	    exit when v_num = 3;
	    /*
	    if v_num = 3 then
	        exit;
	    end if;
	    */
	end loop;
END;
```

- ### While Loop

```sql
DECLARE
	v_num number := 0;
BEGIN
	while v_num <= 100 loop

	    exit when v_num > 40;
	    
	    if v_num = 20 then
	        v_num := v_num + 1;
	        continue;
	    end if;
	    
	    if mod(v_num,10) = 0 then
	        dbms_output.put_line(v_num || ' can be divided by 10.');
	    end if;
	    
	    v_num := v_num + 1;
	    
	end loop;
END;
```

- ### For Loop

```sql
DECLARE
	v_num number := 0;
BEGIN
	for x in 10 .. 13 loop
	    dbms_output.put_line(x);
	end loop;

	for x in reverse 13 .. 15 loop
	    if mod(x,2) = 0 then
	        dbms_output.put_line('even: ' || x);
	    else
	        dbms_output.put_line('odd: ' || x);
	    end if;
	end loop;
END;
```

## Label

A label is an identifier that can be assigned to a statement, such as a loop or a block. It is used to identify a specific block or loop so that it can be referred to in another part of the program.
> By specifying the label name followed by a period and the name of a variable, you can refer to a variable that is declared in a block that is labeled with the specified label.

## Functions

- Syntax:
```sql
CREATE [OR REPLACE] FUNCTION function_name 
  [(parameter_name [IN | OUT | IN OUT] parameter_data_type [, ...])] 
RETURN return_data_type 
[IS | AS]
BEGIN
  -- PL/SQL code for the function
END [function_name];
```
- Keywords Used:
    - `CREATE [OR REPLACE] FUNCTION`: This is the keyword that tells the database that you want to create a new function. The OR REPLACE clause is optional and tells the database to replace the function if it already exists.
    - `IN | OUT | IN OUT`: These are optional keywords that indicate the direction of the parameter.
    - `RETURN`: This is the keyword that indicates the data type that the function returns.
    - `IS | AS`: This keyword separates the declaration section of the function from the executable section.
    - `BEGIN`: This keyword marks the beginning of the executable section of the function.
    - `END [function_name]`: This keyword marks the end of the function. The optional function_name is used to specify the name of the function to be ended.
- Example:
```sql
CREATE OR REPLACE FUNCTION add_numbers(num1 NUMBER, num2 NUMBER) 
RETURN NUMBER
IS
  result NUMBER;
BEGIN
  result := num1 + num2;
  RETURN result;
END add_numbers;
```