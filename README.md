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


--write a SQL query to count the number of employees, 
--the sum of all salary, and 
--difference between the highest salary and lowest salaries by each job id. 

SELECT job_id, COUNT(*) [# of employees], SUM(salary) [Total Salary], MAX(salary) - MIN(salary) AS salary_difference
 FROM employees
 GROUP BY job_id;

 --write a SQL query to count the number of employees worked under each manager
 SELECT manager_id, COUNT(*) [Total Emp]
 FROM employees
 GROUP BY manager_id;

--write a SQL query to find all those employees who are either Programmer or Finance Manager
SELECT [employee_id]
      ,[first_name] +' '+       [last_name]
      ,[email]
      ,[phone_number]
      ,[hire_date]
      ,e.[job_id]
      ,[salary] 
      ,[manager_id]
      ,[department_id], j.job_title
  FROM [HR].[dbo].[employees] e join [HR].[dbo].[jobs] j on e.job_id=j.job_id

  where j.job_title in('Programmer','Finance Manager')


--write a SQL query to find the departments where any manager manages four or more employees. Return department_id.
SELECT DISTINCT department_id
 FROM employees
 GROUP BY department_id, manager_id 
 HAVING COUNT(employee_id) >= 4;

--write a SQL query to compute the average salary of each job ID. Exclude those records where average salary is on or lower than 8000. Return job ID, average salary.
SELECT job_id, AVG(salary) 
 FROM employees 
 GROUP BY job_id 
 HAVING AVG(salary) > 8000;


--write a SQL query to find those job titles where maximum salary falls between 12000 and 18000 (Begin and end values are included.).  
SELECT job_title, max_salary - min_salary AS salary_differences 
 FROM jobs 
 WHERE max_salary BETWEEN 12000 AND 18000;
 
 --write a SQL query to find those employees who receive a higher salary than the employee with ID 163. Return first name, last name.
 SELECT first_name+' '+last_name,salary 
 FROM employees 
 WHERE salary > 
     ( SELECT salary   FROM employees  WHERE employee_id = 107
    );

--write a SQL query to find those employees who earn more than the average salary
SELECT employee_id, first_name, last_name  
 FROM employees  

 WHERE salary >  
( SELECT AVG(salary)     FROM employees 
);

--write a SQL query to find those employees who report to that manager whose first name is ‘Lex’
SELECT first_name, last_name, employee_id, salary   FROM employees  
 WHERE manager_id = 
(SELECT employee_id    FROM employees    WHERE first_name = 'Lex' 
);

--Display the first name and join date of the employees who joined between 1998 and 2000.
SELECT FIRST_NAME, HIRE_DATE FROM EMPLOYEES 
WHERE year(HIRE_DATE) BETWEEN 1998 AND 2000 ORDER BY HIRE_DATE

--Display the departments into which no employee joined in last two years.
SELECT * 
FROM DEPARTMENTS 
WHERE DEPARTMENT_ID NOT IN 
    (SELECT DISTINCT DEPARTMENT_ID 
     FROM EMPLOYEES 
     WHERE DATEDIFF(YEAR, HIRE_DATE, GETDATE()) < 2);


--Display the details of departments in which the max salary is greater than 10000 for employees  
SELECT * FROM DEPARTMENTS
WHERE DEPARTMENT_ID IN 
(SELECT DEPARTMENT_ID FROM EMPLOYEES 
  
 GROUP BY DEPARTMENT_ID
 HAVING MAX(SALARY) >10000)


--Display the details of employees drawing the highest salary in the department.
SELECT e.* FROM employees e
JOIN (
    SELECT department_id, MAX(salary) AS max_salary     FROM employees     GROUP BY department_id
) max_salaries
ON e.department_id = max_salaries.department_id AND e.salary = max_salaries.max_salary;


-- Display third highest salary of all employees

--With Common Table Expression (CTE) to find the third-highest salary with the following query:

WITH RankedSalaries AS (
    SELECT salary, 
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rank
    FROM employees
)
SELECT salary
FROM RankedSalaries
WHERE rank = 3;

--with sub query
SELECT salary
FROM employees
WHERE salary = (
    SELECT DISTINCT salary 
    FROM employees 
    ORDER BY salary DESC 
    OFFSET 2 ROWS FETCH NEXT 1 ROW ONLY
);
 
