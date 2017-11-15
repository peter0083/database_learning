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

```sql
CREATE TABLE Students
  (sid: CHAR(20),
   name: CHAR(20),
   login: CHAR(10),
   age: INTEGER,
   gpa: REAL)
```
