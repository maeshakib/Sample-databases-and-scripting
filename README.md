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
