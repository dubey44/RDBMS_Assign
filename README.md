
RDBMS_Assignment
Repository for RDBMS assignment

1. Create a database named employee, then import data_science_team.csv proj_table.csv and emp_record_table.csv into the employee database from the given resources.
Query:
create database employee;
use employee;

● Subsequently I imported data_science_team.csv, proj_table.csv and emp_record_table.csv into the employee database from the given resources using MySQLWorkbench.

2. Create an ER diagram for the given employee database.
![image](https://user-images.githubusercontent.com/122452570/221153913-526008e0-3224-4f1d-aa64-912e68360ebe.png)


3. Write a query to fetch EMP_ID, FIRST_NAME, LAST_NAME, GENDER, and DEPARTMENT from the employee record table, and make a list of employees and details of their department.
Query:
SELECT emp_id, first_name, last_name, gender, dept
FROM emp_record_table;

![image](https://user-images.githubusercontent.com/122452570/221154049-b392b5c7-c98c-4647-ae3b-a76fb41d0037.png)


4. Write a query to fetch EMP_ID, FIRST_NAME, LAST_NAME, GENDER, DEPARTMENT, and EMP_RATING if the EMP_RATING is:
● less than two
● greater than four
● between two and four

Query:
4a:
SELECT emp_id, first_name, last_name, gender, dept, emp_rating
FROM emp_record_table
WHERE emp_rating<2;

Output:
![image](https://user-images.githubusercontent.com/122452570/221154126-204d6a64-60ef-4b40-8100-6874e8fe849b.png)


4b:
SELECT emp_id, first_name, last_name, gender, dept, emp_rating
FROM emp_record_table
WHERE emp_rating>4;

Output:
![image](https://user-images.githubusercontent.com/122452570/221154190-f5a97965-ac58-4191-9976-03c9f1ba333b.png)


4c:
SELECT emp_id, first_name, last_name, gender, dept, emp_rating
FROM emp_record_table
WHERE emp_rating BETWEEN 2 AND 4;

Output:
![image](https://user-images.githubusercontent.com/122452570/221154275-b488019c-046c-4ab6-b866-7790f3aef355.png)


5. Write a query to concatenate the FIRST_NAME and the LAST_NAME of employees in the Finance department from the employee table and then give the resultant column alias as NAME.
Query:
SELECT CONCAT(first_name, ' ', last_name) as NAME
FROM emp_record_table
WHERE dept='Finance';

Output:
![image](https://user-images.githubusercontent.com/122452570/221154362-768cb3f9-0236-4b74-a193-e26b93c2b410.png)

6. Write a query to list only those employees who have someone reporting to them. Also, show the number of reporters (including the President).
Query:
SELECT mgr.emp_id, mgr.first_name, mgr.last_name, COUNT(e.emp_id)
FROM emp_record_table AS e INNER JOIN emp_record_table AS mgr
ON e.manager_id = mgr.emp_id
GROUP BY mgr.emp_id, mgr.first_name, mgr.last_name;

Output:
![image](https://user-images.githubusercontent.com/122452570/221154488-01cf640c-4821-49b5-888c-d137a46d424e.png)


7. Write a query to list down all the employees from the healthcare and finance departments using union. Take data from the employee record table.
Query:
SELECT emp_id, first_name, last_name, dept
FROM emp_record_table
WHERE dept='Healthcare'
UNION
SELECT emp_id, first_name, last_name, dept
FROM emp_record_table
WHERE dept='Finance';

Output:
![image](https://user-images.githubusercontent.com/122452570/221154589-6ada69f6-83e8-458d-ac30-12377d49f912.png)


8. Write a query to list down employee details such as EMP_ID, FIRST_NAME, LAST_NAME, ROLE, DEPARTMENT, and EMP_RATING grouped by dept. Also include the respective employee rating along with the max emp rating for the department.
Query:
SELECT emp_id, first_name, last_name, role, dept, emp_rating,
max(emp_rating) OVER(PARTITION BY dept) as max_rating_by_dept
FROM emp_record_table;

Output:
![image](https://user-images.githubusercontent.com/122452570/221154634-8ef4949c-bbe9-4e9d-bca5-385f8b2a4ba3.png)

9. Write a query to calculate the minimum and the maximum salary of the employees in each role. Take data from the employee record table.
Query:
SELECT MIN(salary) as min_sal_by_role, MAX(salary) as max_sal_by_role
FROM emp_record_table
GROUP BY role;

Output:
![image](https://user-images.githubusercontent.com/122452570/221154687-4caed78a-6ab0-449f-882d-5ca4bdb63027.png)


10. Write a query to assign ranks to each employee based on their experience. Take data from the employee record table.
Query:
SELECT DISTINCT emp_id, exp, DENSE_RANK() OVER(ORDER BY exp DESC) AS rank_by_exp
FROM emp_record_table;

Output:
![image](https://user-images.githubusercontent.com/122452570/221154742-eded4ee7-4f61-411b-9bbd-512d364a85db.png)



11. Write a query to create a view that displays employees in various countries whose salary is more than six thousand. Take data from the employee record table.
Query:
CREATE OR REPLACE VIEW sal_greater_six_K
AS SELECT emp_id, first_name, last_name, country, salary
FROM emp_record_table
WHERE salary>6000;

SELECT * FROM sal_greater_six_K;

Output:
![image](https://user-images.githubusercontent.com/122452570/221154795-87b52ee4-748d-4c95-9141-c7635c90f94d.png)


12. Write a nested query to find employees with experience of more than ten years. Take data from the employee record table.
Query:
SELECT *
FROM emp_record_table
WHERE emp_id IN
(SELECT emp_id
FROM emp_record_table
WHERE exp>10);

Output:
![image](https://user-images.githubusercontent.com/122452570/221154854-1a885967-66b5-4897-ae10-6a96f2ff2527.png)

13. Write a query to create a stored procedure to retrieve the details of the employees whose experience is more than three years. Take data from the employee record table.
Query:
delimiter $$
create procedure emp_details()
begin
select * from emp_record_table
where exp>3;
end$$
delimiter ;
call emp_details();

Output:
![image](https://user-images.githubusercontent.com/122452570/221154901-844aab8f-5c37-47f4-8f1c-55c6c65b61fe.png)

14. Write a query using stored functions in the project table to check whether the job profile assigned to each employee in the data science team matches the organization’s set standard.
The standard being:
For an employee with experience less than or equal to 2 years assign 'JUNIOR DATA SCIENTIST', For an employee with the experience of 2 to 5 years assign 'ASSOCIATE DATA SCIENTIST',
For an employee with the experience of 5 to 10 years assign 'SENIOR DATA SCIENTIST',
For an employee with the experience of 10 to 12 years assign 'LEAD DATA SCIENTIST',
For an employee with the experience of 12 to 16 years assign 'MANAGER'.
Query:
delimiter $$
create function emp_details() returns tinyint(1) deterministic
begin
declare v_exp int default 0;
declare v_role varchar(50) default "";
declare finished int default 0;
declare dummy_cursor cursor for
select exp, role from emp_record_table;
declare continue handler for not found
set finished=1;
open dummy_cursor;
check_role: loop
fetch dummy_cursor into v_exp, v_role;
if finished = 1 then
leave check_role;
end if;
if (v_exp<=2 and v_role!='JUNIOR DATA SCIENTIST') then
return false;
elseif (v_exp>2 and v_exp<=5 and v_role!='ASSOCIATE DATA SCIENTIST') then
return false;
elseif (v_exp>5 and v_exp<=10 and v_role!='SENIOR DATA SCIENTIST') then
return false;
elseif (v_exp>10 and v_exp<=12 and v_role!='LEAD DATA SCIENTIST') then
return false;
elseif (v_exp>12 and v_exp<=16 and v_role!='MANAGER') then
return false;
end if;

end loop check_role;  
close dummy_cursor;  
return true;  
end$$
delimiter ;

delimiter $$
create procedure helper_procedure()
begin
if emp_details() then
select 'The job profile assigned to each employee
in the data science team matches the organization’s set standard.' as message;
else
select 'The job profile assigned to each employee
in the data science team does not match the organization’s set standard.' as message;
end if;
end$$
delimiter ;
call helper_procedure();

Output:
![image](https://user-images.githubusercontent.com/122452570/221155051-951343ca-108d-4dc4-965e-ffe9d7fdd6e2.png)


15. Create an index to improve the cost and performance of the query to find the employee whose FIRST_NAME is ‘Eric’ in the employee table after checking the execution plan.
Query:
create index ename_index
on emp_record_table(first_name);
select *
from emp_record_table
where first_name = 'Eric';

Output:
![image](https://user-images.githubusercontent.com/122452570/221155241-d3e8b7c8-a24e-479f-92b6-9f118ad05b47.png)

16. Write a query to calculate the bonus for all the employees, based on their ratings and salaries (Use the formula: 5% of salary * employee rating).
Query:
select emp_id, emp_rating, salary, (0.05*salary*emp_rating) as bonus
from emp_record_table;

Output:
![image](https://user-images.githubusercontent.com/122452570/221155305-c479824e-36b6-402d-9403-2d3265af3a08.png)

17. Write a query to calculate the average salary distribution based on the continent and country. Take data from the employee record table.
Query:
select distinct continent, avg(salary) over(partition by continent) as avg_sal_by_continent,
country, avg(salary) over(partition by country) as avg_sal_by_country
from emp_record_table;

Output:
![image](https://user-images.githubusercontent.com/122452570/221155704-05090dc1-151c-41ad-8945-03e2fb7f3a03.png)
