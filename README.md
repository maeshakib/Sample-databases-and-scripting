# Sample-databases-and-scripting

The HR sample database has seven tables:

- The employees table stores the data of employees.
- The jobs table stores the job data including job title and salary range.
- The departments table stores department data.
- The dependents table stores the employee’s dependents.
- The locations table stores the location of the departments of the company.
- The countries table stores the data of countries where the company is doing business.
- The regions table stores the data of regions such as Asia, Europe, America, and the Middle East and Africa. The countries are grouped into regions.


   --write a SQL query to find the details of 'Marketing' department
   SELECT *  FROM departments  WHERE department_name = 'Marketing';
  
  --write a SQL query to find those employees whose last name is "Lorentz". Return first name, last name and department ID.
   SELECT first_name, last_name, department_id  FROM employees  WHERE last_name = 'Lorentz';
  
--write a SQL query to find those employees whose salaries are less than 6000. 
   select [first_name]+ ' ' +[last_name] [Full Name] , salary  FROM [HR].[dbo].[employees] where salary>6000

     -- write a SQL query to find those employees whose first name does not contain the letter ‘M’. Sort the result-set in ascending order by department ID
   SELECT first_name +' ' + last_name as Full_Name, hire_date, salary, department_id
 FROM employees   WHERE first_name NOT LIKE '%M%'  ORDER BY department_id;


 --write a SQL query to find those employees who earn between 8000 and 12000 (Begin and end values are included.) . 
--These employees joined before ‘1987-06-05’ and were not included in the department numbers 40, 120 and 70. 
--Return all fields.
 SELECT *
 FROM employees
  WHERE salary BETWEEN 8000 AND 12000 
        OR  department_id NOT IN (40 , 120 , 70)
          AND   hire_date < '2003-06-05'


--write a SQL query to find those employees whose salaries are not between 7000 and 15000 . Sort the result-set in ascending order by the full name (first and last). Return full name and salary.
 SELECT first_name +' ' + last_name AS Name, salary
 FROM employees
 WHERE salary NOT BETWEEN 7000 AND 15000
 ORDER BY first_name+ ' ' + last_name;


  --write a SQL query to find those employees who were hired between November 5th, 1994 and July 5th, 1998. 
 SELECT first_name +' ' +last_name AS Full_Name, job_id, hire_date
 FROM employees 
 WHERE hire_date BETWEEN '1994-11-05' AND '1998-07-05';

  --write a SQL query to find those employees who work under a manager
 SELECT first_name+ ' ' + last_name AS Full_Name, salary, manager_id
 FROM employees
 WHERE manager_id IS NOT NULL;

  --write a SQL query to find those employees whose first name contains a character 's' in the third position.
 SELECT first_name, last_name, department_id
 FROM employees
 WHERE first_name LIKE '__s%';
