# sql-quries
**1. To find the highest salary of an employee in a SQL database**
---
<small>
SELECT MAX(salary) AS highest_salary
FROM employees;
</small>

---
<small>
SELECT salary
FROM employees
ORDER BY salary DESC
LIMIT 1;
</small>small>

---
<small>
SELECT *
FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
</small>

**2. Second highest salary**

SELECT TOP 1 salary
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees)
ORDER BY salary DESC;

SELECT salary
FROM employees
ORDER BY salary DESC
OFFSET 1 ROWS FETCH NEXT 1 ROWS ONLY;

3. nth Highest Salary

SELECT salary
FROM employees e1
WHERE (
    SELECT COUNT(DISTINCT salary)
    FROM employees e2
    WHERE e2.salary > e1.salary
) = 2; -- Change 2 to N-1 (e.g., for the 5th highest salary, use 4)


4. To delete duplicate records based on specific columns like name and category,

This query keeps the first instance of a duplicate found physically in the table.

DELETE FROM employees e
WHERE e.ctid NOT IN (
    SELECT MIN(e2.ctid)
    FROM employees e2
    GROUP BY e2.name, e2.category
);

5. Find the second-highest salary from an employee table
SELECT DISTINCT Salary
FROM Employee
ORDER BY Salary DESC
LIMIT 1 OFFSET 1;
-- Or using a subquery/CTE with DENSE_RANK() for more complex scenarios

6. Find employees who earn more than their manager.
SELECT E.Name
FROM Employee E
JOIN Employee M ON E.ManagerID = M.EmployeeID
WHERE E.Salary > M.Salary;


7. Find customers who never placed an order.
SELECT C.CustomerName
FROM Customers C
LEFT JOIN Orders O ON C.CustomerID = O.CustomerID
WHERE O.OrderID IS NULL;


8. Write a query to retrieve duplicate records from a table.
SELECT ColumnName, COUNT(ColumnName)
FROM TableName
GROUP BY ColumnName
HAVING COUNT(ColumnName) > 1;

9. "Given a table of Employees and a table of Departments, write an SQL query to find all employees who work in the 'Sales' department
SELECT EmpName
FROM Employees
WHERE DeptID IN (
    SELECT DeptID
    FROM Departments
    WHERE DeptName = 'Sales'
);


10. To calculate the sum of all salaries department-wise
SELECT 
    Department, 
    SUM(Salary) AS Total_Salary
FROM 
    Employees
GROUP BY 
    Department;
11. calculate the average salary department-wise

SELECT 
    Department, 
    AVG(Salary) AS Average_Salary
FROM 
    Employees
GROUP BY 
    Department;


