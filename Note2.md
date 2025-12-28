SQL
----------
1. Find the highest salary in emp table
	- SELECT * FROM emp ORDER BY salary DESC LIMIT 1;
2. Find the second highest salary 
	- SELECT MAX(Salary) FROM Employees 
	WHERE Salary < (SELECT MAX(Salary) FROM Employees);

	- SELECT DISTINCT Salary FROM Employees ORDER BY Salary DESC LIMIT 1 OFFSET 1;
3. Find the nth highest salary
	SELECT DISTINCT Salary
	FROM Employees ORDER BY  Salary DESC
	LIMIT 1 OFFSET (N - 1); -- Replace N with the desired rank (e.g., 3 for the 3rd highest)
	
	SELECT DISTINCT salary
	FROM employees e1
	WHERE n = (
		SELECT COUNT(DISTINCT salary)
		FROM employees e2
		WHERE e2.salary >= e1.salary
	);

4. Find the highest salary dept wise
	SELECT Dept_ID, MAX(Salary) AS Highest_Salary
	FROM  Employees
	GROUP BY Dept_ID;
5. SQL Query for Employee Details (Highest Earner)
	SELECT e.emp_name, e.dep_id, e.salary
	FROM  employees e
	WHERE e.salary = (
			SELECT MAX(salary)
			FROM employees e2
			WHERE e2.dep_id = e.dep_id
		);

5. Find average salary of employee
	SELECT AVG(Salary) AS AverageSalary
	FROM Employees;

6. Find the max salary of a dept
	SELECT dept_id, MAX(salary) AS max_salary
	FROM employees
	GROUP BY dept_id;

7. Find the avg salary of a department
	SELECT Department, AVG(Salary) AS DepartmentAverageSalary
	FROM Employees
	GROUP BY Department;


8. Find all employee and its manager
	SELECT e.employee_name AS Employee, m.employee_name AS Manager
	FROM employees e
	INNER JOIN employees m ON e.manager_id = m.employee_id;
	
	SELECT e.employee_name AS Employee, m.employee_name AS Manager
	FROM employees e
	LEFT JOIN employees m ON e.manager_id = m.employee_id;



9. Find the duplicate employee 
	SELECT email, COUNT(email) AS occurrences
	FROM employees
	GROUP BY email
	HAVING COUNT(email) > 1;
	
	SELECT name, department, COUNT(*) AS occurrences
	FROM employees
	GROUP BY name, department
	HAVING COUNT(*) > 1;
	
	SELECT DISTINCT e1.*
	FROM employees e1
	INNER JOIN employees e2 ON e1.name = e2.name 
		AND e1.email = e2.email 
		AND e1.id <> e2.id;



10. How to delete the duplicate record

	DELETE e1 FROM employees e1
	INNER JOIN employees e2 
	ON e1.name = e2.name AND e1.email = e2.email
	WHERE e1.id > e2.id;

	DELETE FROM employees
	WHERE id NOT IN (
		SELECT MIN(id)
		FROM employees
		GROUP BY name, email
	);

11. Write a SQL query to find employees who have the same manager.
	SELECT 
    e1.manager_id, 
    e1.name AS Employee_1, 
    e2.name AS Employee_2
	FROM employees e1
	INNER JOIN employees e2 
		ON e1.manager_id = e2.manager_id 
		AND e1.employee_id < e2.employee_id
	WHERE e1.manager_id IS NOT NULL;

12. Group All Employees by Their Manager
	SELECT manager_id, name AS employee_name
	FROM employees
	WHERE manager_id IS NOT NULL
	ORDER BY manager_id;

https://skphd.medium.com/top-20-sql-queries-interview-questions-with-answers-56e70e4878d2

https://roadmap.sh/questions/sql-queries


1. What is the difference between WHERE and HAVING?
 WHERE for filtering rows before applying any grouping or aggregation.
	SELECT * FROM Users
	WHERE Age > 18;
	
HAVING to filter groups after performing grouping and aggregation. You apply it to the result of aggregate functions, and it is mostly used with the GROUP BY clause.
	SELECT FirstName, Age FROM Users
	GROUP BY FirstName, Age
	HAVING Age > 30;
	
2. How do you find duplicates in a table?
To find duplicate records, you must first define the criteria for detecting duplicates. 
Is it a combination of two or more columns where you want to detect the duplicates, or are you searching for duplicates within a single column?

The following steps will help you find duplicate data in a table.
	--> Use the GROUP BY clause to group all the rows by the column(s) on which you want to check the duplicate values.
	--> Use the COUNT function in the HAVING command to check if any groups have more than one entry.
	single-column duplicates. 
	--------------------------------------
	SELECT Age, COUNT(Age)
	FROM Users
	GROUP BY Age
	HAVING COUNT(Age) > 1
	
	 multi-column (composite) duplicates 
	 --------------------------------------------------------
	 SELECT FirstName, LastName, COUNT(*) AS dup_count
	FROM Users
	GROUP BY FirstName, LastName
	HAVING COUNT(*) > 1;
	
	After finding duplicates, you might be asked how to delete the duplicates. The query to delete duplicates is shown below using Common Table Expression (CTE) and ROW_NUMBER().
	---------------------------------------------------------------------
	WITH ranked AS (
	SELECT *,
         ROW_NUMBER() OVER (PARTITION BY Age ORDER BY id) AS rn
	FROM Users
	)
	DELETE FROM Users
	WHERE id IN (
	SELECT id
	FROM ranked
	WHERE rn > 1
	);
3. What is the difference between INNER JOIN and LEFT JOIN?
	An INNER JOIN returns only rows with a match in both tables based on the specified join condition. 
	If there are no matching rows, there will be no results. The SQL syntax for an INNER JOIN is shown in the code snippet below.

	SELECT table1.column_name1, table1.column_name2, table2.column_name1, table2.column_name2 FROM table1
	INNER JOIN table2
	ON table1.column_name = table2.column_name
	
	SELECT users.firstName, users.lastName, users.age, cities.name as cityName FROM users
	INNER JOIN cities
	ON users.cityId = cities.id
	
	LEFT JOIN returns all the rows from the left table (table 1) and the matched rows from the right table (table 2). If no matching rows exist in the right table (table 2), then NULL values are returned. 
	
	SELECT table1.column_name1, table1.column_name2, table2.column_name1, table2.column_name2 FROM table1
	LEFT JOIN table2
	ON table1.column_name = table2.column_name 

4. Write a query to find the second highest salary from a table
	SELECT DISTINCT Salary
	FROM Salaries
	ORDER BY Salary DESC
	LIMIT 1 OFFSET 1
	
5. What is the difference between UNION and UNION ALL?
	UNION is used for removing duplicates while UNION ALL keeps all duplicates. 
	UNION is slower compared to UNION ALL because of de-duplication. 
	You use UNION when you want to obtain unique records and UNION ALL when you want every row even if they are repeated.
6. What are indexes and why are they useful?
	Indexes in databases are like the indexes in books. 
	They increase the speed of data retrieval from a database. 
	When you want to read data from a table, instead of going through all the rows of the table, indexes help to go straight to the row you are looking for.
	They improve SELECT queries, improve performance, and make sorting and filtering faster. They also ensure data integrity. 
	There are different types of indexes, which include:
		B-Tree index
		Composite index
		Unique index
		Full text index
		Bitmap index
		Clustered index
		Non-clustered index
7. What is a primary key?
	A primary key is the unique identifier of a row of data in a table.
	You use it to identify each row uniquely, and no two rows can have the same primary key. A primary key column cannot be null.
	No, a table in a relational database can only have one primary key constraint. 
	
	CREATE TABLE users (
	user_id INT PRIMARY KEY,
	name VARCHAR(100),
	phoneNumber VARCHAR(100)
	);

8. What is a foreign key?
	A foreign key is like a bridge between two tables.
	A foreign key in one table is the primary key in another. It is the connector between the two tables.
	A single database table can have multiple foreign keys.
	
9. How does GROUP BY work?
	GROUP BY is a standard SQL command that groups rows with the same value in the specified column.
	You should use with aggregate functions such as COUNT, MIN, MAX, etc.
	SELECT columnName FROM Table
	GROUP BY columnName
	
10. What happens if you SELECT a column not in the GROUP BY clause?
	it will throw an error stating that the column must be in the GROUP BY clause or in an aggregate function. 
	
	SELECT firstName, phoneNumber FROM phoneNumbers 
	GROUP BY phoneNumber   --> will get error
	
	Write a query to COUNT the number of users by country.
	SELECT country, COUNT(country) FROM users
	GROUP BY country
	
11. What happens if you use GROUP BY without an aggregate function?
	If you use the GROUP BY clause without an aggregate function, it is equivalent to using the DISTINCT command. 
	
	SELECT phoneNumber FROM phoneNumbers
	GROUP BY phoneNumber
	
	is equivalent to:
		SELECT DISTINCT phoneNumber FROM phoneNumbers
12. What is the difference between COUNT(*) and COUNT(column_name)?
	The difference is that COUNT(*) counts all the rows of data, including NULL values, 
	while COUNT(column_name) counts only non-NULL values in the specified column.
	
13. What is the difference between a subquery and a JOIN?
	A subquery is a query that is inside another query. You use it for queries that require complex logic. 
	You should use subqueries when you want to use the result of that subquery for another query. 
	
	SELECT firstName,
		(SELECT COUNT(*)
		FROM cities
		WHERE cities.id = users.city_id) AS cityCount
	FROM users;
	
	On the other hand, a JOIN combines two or more tables based on related columns between them. 
	The related column is usually a foreign key.
	You should use JOINS when you want to pull related data from different tables together. 
	
	SELECT firstName, COUNT(*) FROM users
	JOIN cities ON users.city_id = cities.id
	
	A JOIN is faster than a subquery in the following scenarios:
		When you are querying data from multiple tables.
		When you are filtering or joining on index columns.
14. Write a query to find employees earning more than the average salary
		SELECT * FROM employees
		WHERE salary > (SELECT AVG(salary) FROM employees);
		
15. Explain how a correlated subquery works
	A correlated subquery is a subquery that depends on a value from the outer query. 
	This means that the query is evaluated for each row that might be selected in the outer query. 
	
	SELECT name, country_id, salary
	FROM employees em
	WHERE salary > (
	  SELECT AVG(salary) FROM employees
	  country_id = em.country_id);
	  
	 The code above:
		Runs the outer query through each row of the table.
		Takes the country_id from the employees table.
		Iterates through the other rows and does the same calculation.
	This leads to a degrading performance as the data in the table grows.
	You should use a correlated subquery if you want to perform row-specific operations or cannot achieve an operation using JOIN or other aggregate functions.	
