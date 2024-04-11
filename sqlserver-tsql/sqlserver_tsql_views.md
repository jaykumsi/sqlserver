![Tinitiate SQLSERVER Training](images/sqlserver.png)
# SQL SERVER VIEWS

* In SQL SERVER, a **view** is a virtual table that is based on the result of   
  a SQL SELECT statement. Views do not store data themselves, but rather provide a way to organize and present data stored in one or more tables or other views. A database **view** is essentially a saved SQL query, which allows you to retrieve a specific set of data from one or more tables in the database. When a user queries the **view**, the database engine retrieves the data from the underlying tables that make up the view and presents it as if it were a table itself.



# Simple View (On One Table)
   A simple view in SQL Server is a view that meets certain criteria, making it updatable directly through insert, update, or delete operations. For a view to be considered simple, it generally needs to adhere to the following rules:

   Single Base Table: The view must be based on a single underlying base table. It cannot be derived from multiple tables or another view.

   No Aggregates or Grouping: The view cannot contain aggregate functions (like SUM, AVG, COUNT, etc.) or the GROUP BY clause.

   No DISTINCT Keyword: The view's SELECT statement cannot include the DISTINCT keyword.

   No Derived Columns: All columns in the view must be directly from the base table without any expressions or functions applied to them.

   No Set Operations: The view cannot include set operations like UNION, INTERSECT, or EXCEPT.

```SQL
-- Simple View (On One Table)
-- Show All employees from SALES dept
-- -----------------------------------
create or alter view sales_employees_sv
as
select  e.empno,e.ename,e.job,e.mgr,e.hiredate
       ,e.sal,e.commission
from   employees.emp e
where  e.deptno  = 1;
```

# Complex View (With Joins)
 A complex view in SQL Server is a view that involves more advanced features such as joins, subqueries, aggregate functions, or the DISTINCT keyword. These views are typically read-only, meaning you cannot directly insert, update, or delete data through the view.

```sql
-- Complex View (With Joins)
-- Show All employees from SALES dept
-- -----------------------------------
CREATE OR ALTER VIEW sales_employees_cv AS
SELECT e.empno, e.ename, e.job, e.mgr, e.hiredate, e.sal, e.commission, d.deptid, d.dname
FROM employees.emp e
JOIN employees.dept d ON d.deptid = e.deptno
WHERE d.dname = 'SALES';

```

* Select from the view

 ```sql
  select * 
  from   employees.sales_employees_cv se
  where  se.sal < 2000
 ```

* Built-in PROC **sp_helptext** can be used to retrieve VIEW definition.
  
  The built-in stored procedure sp_helptext in SQL Server is used to view the text of the definition of a database object such as a stored procedure, view, function, or trigger. This can be particularly useful when you want to inspect the code of an object or when you need to script out the object's definition

```sql
EXEC sp_helptext sales_employees_sv;
EXEC sp_helptext sales_employees_cv;
```