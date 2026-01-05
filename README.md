# sql-quries
**1. To find the highest salary of an employee in a SQL database**
---
<small>
SELECT MAX(salary) AS highest_salary <br>
FROM employees;<br>
</small>

---
<small>
SELECT salary<br>
FROM employees<br>
ORDER BY salary DESC<br>
LIMIT 1;<br>
</small>small>

---
<small>
SELECT *<br>
FROM employees<br>
WHERE salary = (SELECT MAX(salary) FROM employees);<br>
</small>

**2. Second highest salary**

SELECT TOP 1 salary<br>
FROM employees<br>
WHERE salary < (SELECT MAX(salary) FROM employees)<br>
ORDER BY salary DESC;<br>

SELECT salary<br>
FROM employees<br>
ORDER BY salary DESC<br>
OFFSET 1 ROWS FETCH NEXT 1 ROWS ONLY;<br>

3. nth Highest Salary

SELECT salary<br>
FROM employees e1<br>
WHERE (<br>
    SELECT COUNT(DISTINCT salary)<br>
    FROM employees e2<br>
    WHERE e2.salary > e1.salary<br>
) = 2; -- Change 2 to N-1 (e.g., for the 5th highest salary, use 4)<br>


4. To delete duplicate records based on specific columns like name and category,<br>

This query keeps the first instance of a duplicate found physically in the table.<br>

DELETE FROM employees e<br>
WHERE e.ctid NOT IN (<br>
    SELECT MIN(e2.ctid)<br>
    FROM employees e2<br>
    GROUP BY e2.name, e2.category<br>
);<br>

5. Find the second-highest salary from an employee table<br>
SELECT DISTINCT Salary<br>
FROM Employee<br>
ORDER BY Salary DESC<br>
LIMIT 1 OFFSET 1;<br>
-- Or using a subquery/CTE with DENSE_RANK() for more complex scenarios<br>

6. Find employees who earn more than their manager.<br>
SELECT E.Name<br>
FROM Employee E<br>
JOIN Employee M ON E.ManagerID = M.EmployeeID<br>
WHERE E.Salary > M.Salary;<br>


7. Find customers who never placed an order.<br>
SELECT C.CustomerName<br>
FROM Customers C<br>
LEFT JOIN Orders O ON C.CustomerID = O.CustomerID<br>
WHERE O.OrderID IS NULL;<br>


8. Write a query to retrieve duplicate records from a table.<br>
SELECT ColumnName, COUNT(ColumnName)<br>
FROM TableName<br>
GROUP BY ColumnName<br>
HAVING COUNT(ColumnName) > 1;<br>

9. "Given a table of Employees and a table of Departments, write an SQL query to find all employees who work in the 'Sales' department<br>
SELECT EmpName<br>
FROM Employees<br>
WHERE DeptID IN (<br>
    SELECT DeptID<br>
    FROM Departments<br>
    WHERE DeptName = 'Sales'<br>
);<br>


10. To calculate the sum of all salaries department-wise<br>
SELECT <br>
    Department, <br>
    SUM(Salary) AS Total_Salary<br>
FROM Employees<br>
GROUP BY Department;<br>
11. calculate the average salary department-wise<br>

SELECT <br>
    Department, <br>
    AVG(Salary) AS Average_Salary<br>
FROM Employees<br>
GROUP BY Department;<br>


