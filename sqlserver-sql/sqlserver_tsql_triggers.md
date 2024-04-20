![Tinitiate SQLSERVER Training](images/sqlserver.png)
# SQL SERVER TRIGGERS
In SQL Server, triggers are special types of stored procedures that automatically execute or fire when specific actions occur within a database table (or sometimes a view). They are commonly used for enforcing business rules, validating data, and maintaining an audit trail.

## **Types of Triggers**

### After Triggers (Also known as FOR Triggers):

  Fire after the associated SQL statement (INSERT, UPDATE, DELETE) has executed successfully.
  Useful for actions that require the certainty of a successful operation, such as logging, auditing, or complex business logic.

### Instead Of Triggers:

Fire instead of the associated SQL statement.
Useful for overriding the standard actions of a SQL statement, such as modifying the behavior of an INSERT, UPDATE, or DELETE on a view that involves multiple base tables.

### DML Triggers:

Defined to react to data manipulation language (DML) events, such as INSERT, UPDATE, or DELETE actions on a table or view.

### DDL Triggers:

Defined to react to data definition language (DDL) events, such as CREATE, ALTER, or DROP actions on database objects.

## Components of a Trigger

### Trigger Event: 
  The specific SQL statement that causes the trigger to fire, such as INSERT, UPDATE, or DELETE.

### Trigger Action: 
  The set of SQL statements that are executed when the trigger fires.

Example: Creating an After Trigger for Auditing
Suppose we have two tables: emp (where employee records are stored) and emp_audit (where changes to employee records are logged). Here’s how you might create an after trigger to log any updates made to employee data.

Tables Setup
sql
Copy code
-- Employee table
CREATE TABLE emp (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(255),
    salary DECIMAL(10, 2)
);

-- Audit table
CREATE TABLE emp_audit (
    audit_id INT IDENTITY(1,1) PRIMARY KEY,
    emp_id INT,
    changed_on DATETIME,
    old_salary DECIMAL(10, 2),
    new_salary DECIMAL(10, 2)
);
Creating the Trigger
sql
Copy code
CREATE TRIGGER trg_emp_after_update
ON emp
AFTER UPDATE
AS
BEGIN
    INSERT INTO emp_audit (emp_id, changed_on, old_salary, new_salary)
    SELECT 
        i.emp_id, 
        GETDATE(), 
        d.salary,  -- Old salary
        i.salary   -- New salary
    FROM 
        inserted i
        INNER JOIN deleted d ON i.emp_id = d.emp_id;
END;
Explanation:
Trigger Name: trg_emp_after_update
Trigger Type: AFTER UPDATE
Tables: emp (source table) and emp_audit (audit table)
Functionality:
When an update occurs on the emp table, the trigger fires after the update.
The trigger inserts a record into the emp_audit table. This record includes the employee ID, the time of the change, the old salary (from the deleted pseudo-table), and the new salary (from the inserted pseudo-table).
Trigger Behavior
Inserted and Deleted Tables: SQL Server uses two special pseudo-tables, inserted and deleted, to hold the affected rows during the execution of a trigger for UPDATE statements. inserted contains the new version of the rows, while deleted contains the old versions.
Performance Consideration: Triggers can significantly affect the performance of the operations that cause them to fire because they add extra processing. It's important to ensure that the code within a trigger is efficient and minimized for impact.