Things to note:
- Cardinality = # of rows
- Order of rows is not important
- Order of columns is not important
- another name for a row is a tuple

NULL: a special value
- it may occur in the table
- NULL means unknown

Strengths of relational model
- simple and powerful querying of data
- query optimizer can reorder your query and not changing the answer

Task: create table
- create a table called Students
- 5 attributes
- domain constraints enforced by data type and character limits
```sql
CREATE TABLE Students
  (sid: CHAR(20) PRIMARY KEY,
   name: CHAR(20),
   login: CHAR(10) UNIQUE,
   age: INTEGER,
   gpa: REAL)
```

Task: Drop table
- destorys the relation Students
- the schema info and tuples are deleted
```sql
DROP TABLE Students
```

Task: alter table
```sql
ALTER TABLE Students
  ADD COLUMN firstYear: integer
```

Task: add and delete tuples

```sql
INSERT
INTO Students (sid, name, login, age, gpa)
VALUES (53688, 'Smith', 'smith@ee', 18, 3.2)
```
Delete all the rows completely if the name is "Smith"
```sql
DELETE
FROM Students
WHERE name = 'Smith'
```
What if there is no "Smith" in the table Students?
- you get a "no change" message

Task: update tuples

```sql
UPDATE Students
SET gpa = gpa -1
WHERE name = 'Smith'
```

Can update all tuples unconditionally
```sql
UPDATE Students
SET gpa = gpa - 1
```
