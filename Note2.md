SQL
----------
1. Find the highest salary in emp table <br>
	- SELECT * FROM emp <br>
	ORDER BY salary DESC LIMIT 1; 
2. Find the second highest salary  <br>
	- SELECT MAX(Salary) FROM Employees <br>
	WHERE Salary < (SELECT MAX(Salary) FROM Employees); <br>

	- SELECT DISTINCT Salary FROM Employees <br>
	ORDER BY Salary DESC LIMIT 1 OFFSET 1;
3. Find the nth highest salary  <br>
   
	SELECT DISTINCT Salary <br>
	FROM Employees ORDER BY  Salary DESC <br>
	LIMIT 1 OFFSET (N - 1); -- Replace N with the desired rank (e.g., 3 for the 3rd highest) <br>
	
	SELECT DISTINCT salary  <br>
	FROM employees e1  <br>
	WHERE n = (  <br>
		SELECT COUNT(DISTINCT salary)  <br>
		FROM employees e2   <br>
		WHERE e2.salary >= e1.salary <br>
	);

5. Find the highest salary dept wise  <br>
	SELECT Dept_ID, MAX(Salary) AS Highest_Salary  <br>
	FROM  Employees  <br>
	GROUP BY Dept_ID; 
6. SQL Query for Employee Details (Highest Earner)   <br>
	SELECT e.emp_name, e.dep_id, e.salary <br>
	FROM  employees e  <br>
	WHERE e.salary = (  <br>
			SELECT MAX(salary)  <br>
			FROM employees e2  <br>
			WHERE e2.dep_id = e.dep_id  <br>
		);

5. Find average salary of employee   <br>
	SELECT AVG(Salary) AS AverageSalary  <br>
	FROM Employees;  <br>

6. Find the max salary of a dept  <br>
	SELECT dept_id, MAX(salary) AS max_salary  <br>
	FROM employees  <br>
	GROUP BY dept_id;

7. Find the avg salary of a department  <br>
	SELECT Department, AVG(Salary) AS DepartmentAverageSalary <br>
	FROM Employees  <br>
	GROUP BY Department;


8. Find all employee and its manager  <br>
	SELECT e.employee_name AS Employee, m.employee_name AS Manager <br>
	FROM employees e <br>
	INNER JOIN employees m ON e.manager_id = m.employee_id; <br>
	
	SELECT e.employee_name AS Employee, m.employee_name AS Manager <br>
	FROM employees e <br>
	LEFT JOIN employees m ON e.manager_id = m.employee_id; <br>



9. Find the duplicate employee   <br>
	SELECT email, COUNT(email) AS occurrences <br>
	FROM employees  <br>
	GROUP BY email   <br>
	HAVING COUNT(email) > 1;  <br>
	
	SELECT name, department, COUNT(*) AS occurrences  <br>
	FROM employees   <br>
	GROUP BY name, department  <br>
	HAVING COUNT(*) > 1;  <br>
	
	SELECT DISTINCT e1.*  <br>
	FROM employees e1  <br>
	INNER JOIN employees e2 ON e1.name = e2.name  <br>
		AND e1.email = e2.email <br>
		AND e1.id <> e2.id; 



10. How to delete the duplicate record <br>

	DELETE e1 FROM employees e1 <br>
	INNER JOIN employees e2 <br>
	ON e1.name = e2.name AND e1.email = e2.email  <br>
	WHERE e1.id > e2.id;  <br>

	DELETE FROM employees  <br> 
	WHERE id NOT IN (  <br>
		SELECT MIN(id)  <br>
		FROM employees  <br>
		GROUP BY name, email  <br>
	);

11. Write a SQL query to find employees who have the same manager. <br>
	SELECT <br>
    e1.manager_id, <br>
    e1.name AS Employee_1, <br>
    e2.name AS Employee_2  <br>
	FROM employees e1  <br>
	INNER JOIN employees e2  <br>
		ON e1.manager_id = e2.manager_id <br>
		AND e1.employee_id < e2.employee_id  <br>
	WHERE e1.manager_id IS NOT NULL;

12. Group All Employees by Their Manager  <br> 
	SELECT manager_id, name AS employee_name  <br>
	FROM employees <br>
	WHERE manager_id IS NOT NULL  <br>
	ORDER BY manager_id; <br>

https://skphd.medium.com/top-20-sql-queries-interview-questions-with-answers-56e70e4878d2

https://roadmap.sh/questions/sql-queries


1. What is the difference between WHERE and HAVING? <br>
 WHERE for filtering rows before applying any grouping or aggregation. <br>
	SELECT * FROM Users <br>
	WHERE Age > 18;  <br>
	
HAVING to filter groups after performing grouping and aggregation. You apply it to the result of aggregate functions, and it is mostly used with the GROUP BY clause.<br>
	SELECT FirstName, Age FROM Users <br>
	GROUP BY FirstName, Age  <br>
	HAVING Age > 30;  <br>
	
2. How do you find duplicates in a table?  <br>
To find duplicate records, you must first define the criteria for detecting duplicates.<br>
Is it a combination of two or more columns where you want to detect the duplicates, or are you searching for duplicates within a single column? <br>

The following steps will help you find duplicate data in a table. <br>
	--> Use the GROUP BY clause to group all the rows by the column(s) on which you want to check the duplicate values. <br>
	--> Use the COUNT function in the HAVING command to check if any groups have more than one entry.<br>
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
3. What is the difference between INNER JOIN and LEFT JOIN? <br>
	An INNER JOIN returns only rows with a match in both tables based on the specified join condition.  <br>
	If there are no matching rows, there will be no results. The SQL syntax for an INNER JOIN is shown in the code snippet below. <br>

	SELECT table1.column_name1, table1.column_name2, table2.column_name1, table2.column_name2 FROM table1<br>
	INNER JOIN table2<br>
	ON table1.column_name = table2.column_name<br>
	
	SELECT users.firstName, users.lastName, users.age, cities.name as cityName FROM users<br>
	INNER JOIN cities<br>
	ON users.cityId = cities.id<br>
	
	LEFT JOIN returns all the rows from the left table (table 1) and the matched rows from the right table (table 2). If no matching rows exist in the right table (table 2), then NULL values are returned. <br>
	
	SELECT table1.column_name1, table1.column_name2, table2.column_name1, table2.column_name2 FROM table1<br>
	LEFT JOIN table2<br>
	ON table1.column_name = table2.column_name <br>

4. Write a query to find the second highest salary from a table<br>
	SELECT DISTINCT Salary<br>
	FROM Salaries<br>
	ORDER BY Salary DESC<br>
	LIMIT 1 OFFSET 1<br>
	
5. What is the difference between UNION and UNION ALL?<br>
	UNION is used for removing duplicates while UNION ALL keeps all duplicates. <br>
	UNION is slower compared to UNION ALL because of de-duplication. <br>
	You use UNION when you want to obtain unique records and UNION ALL when you want every row even if they are repeated.<br>
6. What are indexes and why are they useful?<br>
	Indexes in databases are like the indexes in books. <br>
	They increase the speed of data retrieval from a database. <br>
	When you want to read data from a table, instead of going through all the rows of the table, indexes help to go straight to the row you are looking for.<br>
	They improve SELECT queries, improve performance, and make sorting and filtering faster. They also ensure data integrity. <br>
	There are different types of indexes, which include:<br>
		B-Tree index<br>
		Composite index<br>
		Unique index<br>
		Full text index<br>
		Bitmap index<br>
		Clustered index<br>
		Non-clustered index<br>
7. What is a primary key?<br>
	A primary key is the unique identifier of a row of data in a table.<br>
	You use it to identify each row uniquely, and no two rows can have the same primary key. A primary key column cannot be null.<br>
	No, a table in a relational database can only have one primary key constraint. <br>
	
	CREATE TABLE users (<br>
	user_id INT PRIMARY KEY,<br>
	name VARCHAR(100),<br>
	phoneNumber VARCHAR(100)<br>
	);

8. What is a foreign key?<br>
	A foreign key is like a bridge between two tables.<br>
	A foreign key in one table is the primary key in another. It is the connector between the two tables.<br>
	A single database table can have multiple foreign keys.<br>
	
9. How does GROUP BY work?<br>
	GROUP BY is a standard SQL command that groups rows with the same value in the specified column.<br>
	You should use with aggregate functions such as COUNT, MIN, MAX, etc.<br>
	SELECT columnName FROM Table<br>
	GROUP BY columnName<br>
	
10. What happens if you SELECT a column not in the GROUP BY clause?<br>
	it will throw an error stating that the column must be in the GROUP BY clause or in an aggregate function. <br>
	
	SELECT firstName, phoneNumber FROM phoneNumbers <br>
	GROUP BY phoneNumber   --> will get error<br>
	
	Write a query to COUNT the number of users by country.<br>
	SELECT country, COUNT(country) FROM users<br>
	GROUP BY country<br>
	
11. What happens if you use GROUP BY without an aggregate function?<br>
	If you use the GROUP BY clause without an aggregate function, it is equivalent to using the DISTINCT command. <br>
	
	SELECT phoneNumber FROM phoneNumbers<br>
	GROUP BY phoneNumber<br>
	
	is equivalent to:<br>
		SELECT DISTINCT phoneNumber FROM phoneNumbers<br>
12. What is the difference between COUNT(*) and COUNT(column_name)?<br>
	The difference is that COUNT(*) counts all the rows of data, including NULL values, <br>
	while COUNT(column_name) counts only non-NULL values in the specified column.<br>
	
13. What is the difference between a subquery and a JOIN?<br>
	A subquery is a query that is inside another query. You use it for queries that require complex logic. <br>
	You should use subqueries when you want to use the result of that subquery for another query. <br>
	
	SELECT firstName,<br>
		(SELECT COUNT(*)<br>
		FROM cities<br>
		WHERE cities.id = users.city_id) AS cityCount<br>
	FROM users;<br>
	
	On the other hand, a JOIN combines two or more tables based on related columns between them. <br>
	The related column is usually a foreign key.<br>
	You should use JOINS when you want to pull related data from different tables together. <br>
	
	SELECT firstName, COUNT(*) FROM users<br>
	JOIN cities ON users.city_id = cities.id <br>
	
	A JOIN is faster than a subquery in the following scenarios: <br>
		When you are querying data from multiple tables. <br>
		When you are filtering or joining on index columns. <br>
14. Write a query to find employees earning more than the average salary <br>
		SELECT * FROM employees <br>
		WHERE salary > (SELECT AVG(salary) FROM employees); <br>
		
15. Explain how a correlated subquery works <br>
	A correlated subquery is a subquery that depends on a value from the outer query. <br>
	This means that the query is evaluated for each row that might be selected in the outer query. <br>
	
	SELECT name, country_id, salary<br>
	FROM employees em<br>
	WHERE salary > (<br>
	  SELECT AVG(salary) FROM employees<br>
	  country_id = em.country_id);<br>
	  
	 The code above:<br>
		Runs the outer query through each row of the table.<br>
		Takes the country_id from the employees table.<br>
		Iterates through the other rows and does the same calculation. <br>
	This leads to a degrading performance as the data in the table grows. <br>
	You should use a correlated subquery if you want to perform row-specific operations or cannot achieve an operation using JOIN or other aggregate functions.	
