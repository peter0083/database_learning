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
   gpa: REAL);
   
CREATE TABLE Courses
  (cid: CHAR(5) PRIMARY KEY,
   name: CHAR(20));
```
in sqlite:
```sql
CREATE TABLE Students
  (sid CHAR(20) PRIMARY KEY,
   name CHAR(20),
   login CHAR(10) UNIQUE,
   age INTEGER,
   gpa REAL);
   
CREATE TABLE Courses
  (cid CHAR(5) PRIMARY KEY,
   name CHAR(20));
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

Task: insert multiple tuples
```sql
INSERT
INTO Students (sid, name, login, age, gpa)
VALUES (52398, 'Peter', 'peter@ubc', 34, 4.0),
       (54343, 'Gary', 'gary@sfu', 32, 4.0),
       (52377, 'Charles', 'charles@ubc', 28, 3.6)
``` 

Delete all the rows completely if the name is "Smith" (exact match only)
- Student with the name "John Smith" will not be affected.
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
Recall: 
- integrity constraints, ICs, include domain constraints 
- a legal instance of a relation is one that satisfies all specific ICs

Where do integrity constraints come from?
- ICs are based on real-world semantics
- if you are only given a table instance, you cannot always guess all the constraints
- IC is a statement about all possible instances
- the more ICs we capture, less likely the model will fail

Primary key constraints
- given a set S = {S1, S2, ... , Sm}
1. no two distinct tuplescan have the same values in all the key fields AND
2. no subset of S is itself a key (according to 1)
- primary key cannot be NULL!

Primary key can be a combo of two attributes
- ie. primary key = name and address
- it means that "you can have duplicate names. you can have duplicate addresses. but! you cannot have a duplicate of the name+address combination"

Super key
- super keys are additional attributes are NOT really needed to serve the role of a key

Candidate key
- a candidate to be a primary key
- `UNIQUE` is SQL's way to say that it is a candidate key

Task: spot the silly constraint
note: cid is course ID
```sql
CREATE TABLE Enrolled
(sid CHAR(20),
 cid CHAR(20),
 grade CHAR(2),
 PRIMARY KEY (sid),
 UNIQUE (cid, grade));
```
-  `UNIQUE (cid, grade)` means that no two student can get the same grade! bizarre!

Foreign keys: referential integrity
- think of foreign keys as logical pointers
- ie. you should an existing course from a course table and an existing student from a student table

example: foreign key pointing to Students table
```sql
CREATE TABLE Enrolled
  (sid CHAR(20),
   cid CHAR(20),
   grade CHAR(2),
   PRIMARY KEY (sid, cid),
   FOREIGN KEY (sid) REFERENCE Students,
   FOREIGN KEY (cid) REFERENCE Courses);
``` 
Enforcing foreign key/referential integrity
- if you try to insert a non-existing student into an Enrolled table ---> reject!
- what if a Students tuple is deleted
  - also delete all Enrolled tuples that refers to it?
  - disallow  deletion of this particular Students tuple?
  
```sql
CREATE TABLE Enrolled
  (sid CHAR(20),
   cid CHAR(20),
   grade CHAR(2),
   PRIMARY KEY (sid, cid),
   FOREIGN KEY (sid) 
      REFERENCES Students(sid)
      ON DELETE CASCADE
      ON UPDATE CASCADE,
   FOREIGN KEY (cid) 
      REFERENCES Courses(cid)
      ON DELETE CASCADE
      ON UPDATE CASCADE);
``` 
Putting everything together

```sql
CREATE TABLE Courses
  (name CHAR(20),
   cid CHAR(5) PRIMARY KEY);

CREATE TABLE Students
  (sid CHAR(20) PRIMARY KEY,
   name CHAR(20),
   login CHAR(10) UNIQUE,
   age INTEGER,
   gpa REAL);

INSERT
INTO Students (sid, name, login, age, gpa)
VALUES (53688, 'Peter', 'peter@ubc', 34, 4.0),
       (54343, 'Gary', 'gary@sfu', 32, 4.0),
       (52377, 'Charles', 'charles@ubc', 28, 3.6);

INSERT
INTO Courses (cid, name)
VALUES (522, 'DSCI'),
       (571, 'DSCI');
       
CREATE TABLE Enrolled
  (sid CHAR(20),
   cid CHAR(20),
   grade CHAR(2),
   PRIMARY KEY (sid, cid),
   FOREIGN KEY (sid) 
      REFERENCES Students(sid)
      ON DELETE CASCADE
      ON UPDATE CASCADE,
   FOREIGN KEY (cid) 
      REFERENCES Courses(cid)
      ON DELETE CASCADE
      ON UPDATE CASCADE);
      
INSERT
INTO Enrolled (sid, cid, grade)
VALUES (53688, 571, 97),
       (5237, 571, 68);

SELECT * FROM Students;

SELECT * FROM Courses;

SELECT * FROM Enrolled;
```
