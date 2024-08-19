# Sample-databases-and-scripting

The HR sample database has seven tables:

- The employees table stores the data of employees.
- The jobs table stores the job data including job title and salary range.
- The departments table stores department data.
- The dependents table stores the employee’s dependents.
- The locations table stores the location of the departments of the company.
- The countries table stores the data of countries where the company is doing business.
- The regions table stores the data of regions such as Asia, Europe, America, and the Middle East and Africa. The countries are grouped into regions.


#### write a SQL query to find the details of 'Marketing' department

```sql
   SELECT *  FROM departments  WHERE department_name = 'Marketing'
```

#### write a SQL query to find those employees whose last name is "Lorentz". Return first name, last name and department ID.
  ```sql
   SELECT first_name, last_name, department_id  FROM employees  WHERE last_name = 'Lorentz';
```
#### write a SQL query to find those employees whose salaries are less than 6000. 
```sql
   select [first_name]+ ' ' +[last_name] [Full Name] , salary  FROM [HR].[dbo].[employees] where salary>6000
```


#### write a SQL query to find those employees whose first name does not contain the letter ‘M’. Sort the result-set in ascending order by department ID
```sql
   SELECT first_name +' ' + last_name as Full_Name, hire_date, salary, department_id
 FROM employees   WHERE first_name NOT LIKE '%M%'  ORDER BY department_id;
```

#### write a SQL query to find those employees who earn between 8000 and 12000 (Begin and end values are included.) .

#### These employees joined before ‘1987-06-05’ and were not included in the department numbers 40, 120 and 70. 
 
```sql
 SELECT *
 FROM employees
  WHERE salary BETWEEN 8000 AND 12000 
        OR  department_id NOT IN (40 , 120 , 70)
          AND   hire_date < '2003-06-05'
```

#### write a SQL query to find those employees whose salaries are not between 7000 and 15000 . Sort the result-set in ascending order by the full name (first and last). Return full name and salary.
```sql
 SELECT first_name +' ' + last_name AS Name, salary
 FROM employees
 WHERE salary NOT BETWEEN 7000 AND 15000
 ORDER BY first_name+ ' ' + last_name;
```


#### write a SQL query to find those employees who were hired between November 5th, 1994 and July 5th, 1998.
```sql
 SELECT first_name +' ' +last_name AS Full_Name, job_id, hire_date
 FROM employees 
 WHERE hire_date BETWEEN '1994-11-05' AND '1998-07-05';
```


#### write a SQL query to find those employees who work under a manager
```sql
 SELECT first_name+ ' ' + last_name AS Full_Name, salary, manager_id
 FROM employees
 WHERE manager_id IS NOT NULL;
```

#### write a SQL query to find those employees whose first name contains a character 's' in the third position.
```sql
 SELECT first_name, last_name, department_id
 FROM employees
 WHERE first_name LIKE '__s%';
```

#### write a SQL query to count the number of employees, 
 the sum of all salary, and 
--difference between the highest salary and lowest salaries by each job id. 
```sql
SELECT job_id, COUNT(*) [# of employees], SUM(salary) [Total Salary], MAX(salary) - MIN(salary) AS salary_difference
 FROM employees
 GROUP BY job_id;
```
#### write a SQL query to count the number of employees worked under each manager
```sql
 SELECT manager_id, COUNT(*) [Total Emp]
 FROM employees
 GROUP BY manager_id;
```
#### write a SQL query to find all those employees who are either Programmer or Finance Manager
```sql
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
```

#### write a SQL query to find the departments where any manager manages four or more employees. Return department_id.
```sql
SELECT DISTINCT department_id
 FROM employees
 GROUP BY department_id, manager_id 
 HAVING COUNT(employee_id) >= 4;
```

#### write a SQL query to compute the average salary of each job ID. Exclude those records where average salary is on or lower than 8000. Return job ID, average salary.
```sql
SELECT job_id, AVG(salary) 
 FROM employees 
 GROUP BY job_id 
 HAVING AVG(salary) > 8000;
```

#### write a SQL query to find those job titles where maximum salary falls between 12000 and 18000 (Begin and end values are included.).
```sql
SELECT job_title, max_salary - min_salary AS salary_differences 
 FROM jobs 
 WHERE max_salary BETWEEN 12000 AND 18000;
 ```

#### write a SQL query to find those employees who receive a higher salary than the employee with ID 163. Return first name, last name.
```sql
 SELECT first_name+' '+last_name,salary 
 FROM employees 
 WHERE salary > 
     ( SELECT salary   FROM employees  WHERE employee_id = 107
    );
```
#### write a SQL query to find those employees who earn more than the average salary
```sql
SELECT employee_id, first_name, last_name  
 FROM employees  

 WHERE salary >  
( SELECT AVG(salary)     FROM employees 
);
```

##### write a SQL query to find those employees who report to that manager whose first name is ‘Lex’

```sql
SELECT first_name, last_name, employee_id, salary   FROM employees  
 WHERE manager_id = 
(SELECT employee_id    FROM employees    WHERE first_name = 'Lex' 
);
```

#### Display the first name and join date of the employees who joined between 1998 and 2000.
```sql
SELECT FIRST_NAME, HIRE_DATE FROM EMPLOYEES 
WHERE year(HIRE_DATE) BETWEEN 1998 AND 2000 ORDER BY HIRE_DATE
```

#### Display the departments into which no employee joined in last two years.
```sql
SELECT * 
FROM DEPARTMENTS 
WHERE DEPARTMENT_ID NOT IN 
    (SELECT DISTINCT DEPARTMENT_ID 
     FROM EMPLOYEES 
     WHERE DATEDIFF(YEAR, HIRE_DATE, GETDATE()) < 2);
```

#### Display the details of departments in which the max salary is greater than 10000 for employees

```sql
SELECT * FROM DEPARTMENTS
WHERE DEPARTMENT_ID IN 
(SELECT DEPARTMENT_ID FROM EMPLOYEES 
  
 GROUP BY DEPARTMENT_ID
 HAVING MAX(SALARY) >10000)
```

#### Display the details of employees drawing the highest salary in the department.
```sql
SELECT e.* FROM employees e
JOIN (
    SELECT department_id, MAX(salary) AS max_salary     FROM employees     GROUP BY department_id
) max_salaries
ON e.department_id = max_salaries.department_id AND e.salary = max_salaries.max_salary;
```

### Display third highest salary of all employees

#### With Common Table Expression (CTE) to find the third-highest salary with the following query:

```sql
WITH RankedSalaries AS (
    SELECT salary, 
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rank
    FROM employees
)
SELECT salary
FROM RankedSalaries
WHERE rank = 3;
```
--with sub query

```sql
SELECT salary
FROM employees
WHERE salary = (
    SELECT DISTINCT salary 
    FROM employees 
    ORDER BY salary DESC 
    OFFSET 2 ROWS FETCH NEXT 1 ROW ONLY
);
```

 ### How to Find All Employees Under Each Manager in SQL
 ##### with left join 
```sql
SELECT 
    e.employee_id AS Employee_ID,
    e.first_name AS Employee_First_Name,
    e.last_name AS Employee_Last_Name,
    m.employee_id AS Manager_ID,
    m.first_name AS Manager_First_Name,
    m.last_name AS Manager_Last_Name
FROM 
    employees e
LEFT JOIN 
    employees m ON e.manager_id = m.employee_id
ORDER BY 
    e.manager_id, e.employee_id;
```
 ##### with recursive query
```sql
	WITH EmployeeHierarchy AS (
    -- Anchor member: Select top-level employees (employees with no manager)
    SELECT 
        employee_id, 
        first_name, 
        last_name, 
        manager_id, 
        1 AS hierarchy_level
    FROM 
        employees
    WHERE 
        manager_id IS NULL

    UNION ALL

    -- Recursive member: Select employees and join them with their managers from the CTE
    SELECT 
        e.employee_id, 
        e.first_name, 
        e.last_name, 
        e.manager_id, 
        eh.hierarchy_level + 1 AS hierarchy_level
    FROM 
        employees e
    INNER JOIN 
        EmployeeHierarchy eh ON e.manager_id = eh.employee_id
)
SELECT 
    employee_id, 
    first_name, 
    last_name, 
    manager_id, 
    hierarchy_level
FROM 
    EmployeeHierarchy
ORDER BY 
    hierarchy_level, manager_id, employee_id;
 ```
#### Retrieve the top 2 highest-paid employees for each department, sorted by salary in descending order.
```sql
WITH RankedEmployees AS (
  SELECT
    employee_id,
    department_id,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rank
  FROM employees
)

SELECT
  employee_id,
  department_id,
  salary
FROM RankedEmployees
WHERE rank <= 2
ORDER BY department_id, rank;
```
####  Query to Find Employees Who Do Not Have Any Dependents:
```sql
SELECT e.employee_id, e.first_name, e.last_name
FROM employees e
LEFT JOIN dependents d ON e.employee_id = d.employee_id
WHERE d.dependent_id IS NULL;
 ```

####  Query to Get the Average Salary by Job Title:
```sql 
SELECT j.job_title, AVG(e.salary) AS average_salary
FROM employees e
JOIN jobs j ON e.job_id = j.job_id
GROUP BY j.job_title
ORDER BY average_salary DESC;
```
#### Query to Find the Top 3 Highest Salaries in the Company:

```sql
SELECT DISTINCT top 3 salary
FROM employees
ORDER BY salary DESC
 ```
 

#### Query to Find Employees Who Work in the Same Location as Their Manager:

```sql
SELECT e.employee_id, e.first_name, e.last_name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN locations l ON d.location_id = l.location_id
JOIN employees m ON e.manager_id = m.employee_id AND e.department_id = m.department_id
WHERE e.department_id = m.department_id
ORDER BY e.employee_id;
```
#### Query to Find Employees Working in Departments Located in a Specific Country (e.g., "United States"):
```sql
SELECT e.employee_id, e.first_name, e.last_name, d.department_name, c.country_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN locations l ON d.location_id = l.location_id
JOIN countries c ON l.country_id = c.country_id
WHERE c.country_name = 'United States';
```
#### Query to List All Jobs with the Number of Employees in Each Job Title:

```sql
SELECT j.job_title, COUNT(e.employee_id) AS num_employees
FROM jobs j
LEFT JOIN employees e ON j.job_id = e.job_id
GROUP BY j.job_title
ORDER BY num_employees DESC;
```
 #### Query to Get the Total Number of Employees, Grouped by Country:

 

SELECT c.country_name, COUNT(e.employee_id) AS total_employees
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN locations l ON d.location_id = l.location_id
JOIN countries c ON l.country_id = c.country_id
GROUP BY c.country_name
ORDER BY total_employees DESC;
 ```
#### Query to Get the Average, Minimum, and Maximum Salary by Department:

 
SELECT d.department_name, 
       AVG(e.salary) AS average_salary, 
       MIN(e.salary) AS minimum_salary, 
       MAX(e.salary) AS maximum_salary
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY d.department_name;
 ```
#### Query to List Employees with Salaries Above the Department Average:

 

SELECT e.employee_id, e.first_name, e.last_name, e.salary, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
)
ORDER BY e.salary DESC;
 ```
#### Query to Get the Number of Employees Hired Each Year:

 ```sql

SELECT YEAR(hire_date) AS hire_year, COUNT(employee_id) AS num_employees
FROM employees
GROUP BY YEAR(hire_date)
ORDER BY hire_year;
 ```
#### Query to Identify Departments with No Employees:

 ```sql

SELECT d.department_name
FROM departments d
LEFT JOIN employees e ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
 ```
#### Query to Find the Manager with the Most Direct Reports:

 ```sql

SELECT m.employee_id, m.first_name, m.last_name, COUNT(e.employee_id) AS num_reports
FROM employees m
JOIN employees e ON m.employee_id = e.manager_id
GROUP BY m.employee_id, m.first_name, m.last_name
ORDER BY num_reports DESC;
 ```
#### Query to Get the Distribution of Employees by Job Title and Department:

 ```sql

SELECT j.job_title, d.department_name, COUNT(e.employee_id) AS num_employees
FROM employees e
JOIN jobs j ON e.job_id = j.job_id
JOIN departments d ON e.department_id = d.department_id
GROUP BY j.job_title, d.department_name
ORDER BY d.department_name, j.job_title;
 ```
#### Query to List Employees Who Have Been with the Company for More Than 30 Years:

 ```sql

SELECT e.employee_id, e.first_name, e.last_name, e.hire_date, DATEDIFF(YEAR, e.hire_date, GETDATE()) AS years_with_company
FROM employees e
WHERE DATEDIFF(YEAR, e.hire_date, GETDATE()) > 30
ORDER BY years_with_company DESC;
 ```
#### Query to Find Departments with an Average Salary Greater Than a Specified Amount (e.g., $70,000):

 ```sql

SELECT d.department_name, AVG(e.salary) AS average_salary
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
HAVING AVG(e.salary) > 70000
ORDER BY average_salary DESC;
 ```
#### Query to Identify the Employee(s) with the Longest Tenure:

 ```sql

SELECT top 1 e.employee_id, e.first_name, e.last_name, e.hire_date, DATEDIFF(DAY, e.hire_date, GETDATE()) AS days_with_company
FROM employees e
ORDER BY days_with_company DESC
 
 ```
#### Query to Calculate the Total Compensation (Salary) Paid Out by Department:

 ```sql

SELECT d.department_name, SUM(e.salary) AS total_compensation
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY total_compensation DESC;
 ```
#### Query to Get the List of Employees Who Share the Same Job Title as Their Manager:

 ```sql

SELECT e.employee_id, e.first_name, e.last_name, e.job_id, j.job_title
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id
JOIN jobs j ON e.job_id = j.job_id
WHERE e.job_id = m.job_id
ORDER BY e.job_id;
 ```
#### Query to Generate a Report of Employees by Region:

 ```sql
SELECT r.region_name, c.country_name, COUNT(e.employee_id) AS num_employees
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN locations l ON d.location_id = l.location_id
JOIN countries c ON l.country_id = c.country_id
JOIN regions r ON c.region_id = r.region_id
GROUP BY r.region_name, c.country_name
ORDER BY r.region_name, c.country_name;
 ```
 #### Pivoting to Show Employee Count by Hire Year and Department:
 ```sql
  SELECT * FROM (
    SELECT 
        YEAR(e.hire_date) AS hire_year, 
        d.department_name, 
        COUNT(e.employee_id) AS num_employees
    FROM 
        employees e
    JOIN 
        departments d ON e.department_id = d.department_id
    GROUP BY 
        YEAR(e.hire_date), d.department_name
) AS SourceTable
PIVOT (
    SUM(num_employees)
    FOR hire_year IN ([1987], [1988], [1989], [1990],  [1991], [1992] ,[1993], [1994], [1995], [1996], [1997],[1998], [1999], [2000])
) AS PivotTable
ORDER BY department_name;
```
 
