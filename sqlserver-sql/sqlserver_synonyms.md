# Synonyms

In SQL Server, synonyms are database objects that provide an alternative name for another database object, known as the base object. They can be used to reference tables, views, stored procedures, and other SQL Server objects. Synonyms help simplify SQL queries, enhance manageability, improve abstraction of underlying data sources, and facilitate operations like refactoring database objects without disrupting client applications.

* Benefits of Using Synonyms
**Simplification:** Synonyms can simplify the SQL syntax, making it easier to write and understand, especially when dealing with complex or long object names.

**Abstraction:** By using synonyms, you can abstract the underlying object names and structures from your applications. This is useful for security and when changing underlying objects without affecting application code.

**Flexibility:** Synonyms provide an additional layer of abstraction that can make it easier to switch out underlying tables or procedures during migrations or upgrades.

**Encapsulation:** They help encapsulate the location details of the base object, which can be particularly useful in distributed databases or when working across different schemas or databases.

## Creating a Synonym
To create a synonym in SQL Server, you use the CREATE SYNONYM statement. Here’s the syntax:

```sql
    CREATE SYNONYM synonym_name FOR object_name;
```
Example
Suppose you have a table named EmployeeDetails located in the schema dbo of a database named HRDatabase. You can create a synonym for it like this:

```sql
    CREATE SYNONYM EmpDetails FOR HRDatabase.dbo.EmployeeDetails;
```

Now, you can use EmpDetails in your SQL queries, instead of HRDatabase.dbo.EmployeeDetails.

## Using a Synonym
Once a synonym is created, you can use it in SQL queries just like you would use the name of the base object:

```sql
    SELECT * FROM EmpDetails WHERE DepartmentID = 101;
```
## Dropping a Synonym
If you need to remove a synonym, you use the DROP SYNONYM statement. It's straightforward:

```sql
    DROP SYNONYM EmpDetails;
```

## Modifying a Synonym
SQL Server does not allow you to directly modify a synonym. If you need to change a synonym (for example, to point it to a different base object), you must first drop the existing synonym and then recreate it with the new definition.
