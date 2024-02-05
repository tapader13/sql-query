# SQL_Query

![Screenshot 2024-02-04 110200](https://github.com/tapader13/sql-query/assets/125242130/17b41e17-fb54-4611-8e3f-303b1e930f3e)

```sql
create database university;
use university;
create table classroom
	(building		varchar(15),
	 room_number		varchar(7),
	 capacity		numeric(4,0),
	 primary key (building, room_number)
	);

create table department
	(dept_name		varchar(20),
	 building		varchar(15),
	 budget		        numeric(12,2) check (budget > 0),
	 primary key (dept_name)
	);

create table course
	(course_id		varchar(8),
	 title			varchar(50),
	 dept_name		varchar(20),
	 credits		numeric(2,0) check (credits > 0),
	 primary key (course_id),
	 foreign key (dept_name) references department (dept_name)
		on delete set null
	);

create table instructor
	(ID			varchar(5),
	 name			varchar(20) not null,
	 dept_name		varchar(20),
	 salary			numeric(8,2) check (salary > 29000),
	 primary key (ID),
	 foreign key (dept_name) references department (dept_name)
		on delete set null
	);

create table section
	(course_id		varchar(8),
         sec_id			varchar(8),
	 semester		varchar(6)
		check (semester in ('Fall', 'Winter', 'Spring', 'Summer')),
	 year			numeric(4,0) check (year > 1701 and year < 2100),
	 building		varchar(15),
	 room_number		varchar(7),
	 time_slot_id		varchar(4),
	 primary key (course_id, sec_id, semester, year),
	 foreign key (course_id) references course (course_id)
		on delete cascade,
	 foreign key (building, room_number) references classroom (building, room_number)
		on delete set null
	);

create table teaches
	(ID			varchar(5),
	 course_id		varchar(8),
	 sec_id			varchar(8),
	 semester		varchar(6),
	 year			numeric(4,0),
	 primary key (ID, course_id, sec_id, semester, year),
	 foreign key (course_id, sec_id, semester, year) references section (course_id, sec_id, semester, year)
		on delete cascade,
	 foreign key (ID) references instructor (ID)
		on delete cascade
	);

create table student
	(ID			varchar(5),
	 name			varchar(20) not null,
	 dept_name		varchar(20),
	 tot_cred		numeric(3,0) check (tot_cred >= 0),
	 primary key (ID),
	 foreign key (dept_name) references department (dept_name)
		on delete set null
	);

create table takes
	(ID			varchar(5),
	 course_id		varchar(8),
	 sec_id			varchar(8),
	 semester		varchar(6),
	 year			numeric(4,0),
	 grade		        varchar(2),
	 primary key (ID, course_id, sec_id, semester, year),
	 foreign key (course_id, sec_id, semester, year) references section (course_id, sec_id, semester, year)
		on delete cascade,
	 foreign key (ID) references student (ID)
		on delete cascade
	);

create table advisor
	(s_ID			varchar(5),
	 i_ID			varchar(5),
	 primary key (s_ID),
	 foreign key (i_ID) references instructor (ID)
		on delete set null,
	 foreign key (s_ID) references student (ID)
		on delete cascade
	);

create table time_slot
	(time_slot_id		varchar(4),
	 day			varchar(1),
	 start_hr		numeric(2) check (start_hr >= 0 and start_hr < 24),
	 start_min		numeric(2) check (start_min >= 0 and start_min < 60),
	 end_hr			numeric(2) check (end_hr >= 0 and end_hr < 24),
	 end_min		numeric(2) check (end_min >= 0 and end_min < 60),
	 primary key (time_slot_id, day, start_hr, start_min)
	);

create table prereq
	(course_id		varchar(8),
	 prereq_id		varchar(8),
	 primary key (course_id, prereq_id),
	 foreign key (course_id) references course (course_id)
		on delete cascade,
	 foreign key (prereq_id) references course (course_id)
	);

    delete from prereq;
delete from time_slot;
delete from advisor;
delete from takes;
delete from student;
delete from teaches;
delete from section;
delete from instructor;
delete from course;
delete from department;
delete from classroom;
insert into classroom values ('Packard', '101', '500');
insert into classroom values ('Painter', '514', '10');
insert into classroom values ('Taylor', '3128', '70');
insert into classroom values ('Watson', '100', '30');
insert into classroom values ('Watson', '120', '50');
insert into department values ('Biology', 'Watson', '90000');
insert into department values ('Comp. Sci.', 'Taylor', '100000');
insert into department values ('Elec. Eng.', 'Taylor', '85000');
insert into department values ('Finance', 'Painter', '120000');
insert into department values ('History', 'Painter', '50000');
insert into department values ('Music', 'Packard', '80000');
insert into department values ('Physics', 'Watson', '70000');
insert into course values ('BIO-101', 'Intro. to Biology', 'Biology', '4');
insert into course values ('BIO-301', 'Genetics', 'Biology', '4');
insert into course values ('BIO-399', 'Computational Biology', 'Biology', '3');
insert into course values ('CS-101', 'Intro. to Computer Science', 'Comp. Sci.', '4');
insert into course values ('CS-190', 'Game Design', 'Comp. Sci.', '4');
insert into course values ('CS-315', 'Robotics', 'Comp. Sci.', '3');
insert into course values ('CS-319', 'Image Processing', 'Comp. Sci.', '3');
insert into course values ('CS-347', 'Database System Concepts', 'Comp. Sci.', '3');
insert into course values ('EE-181', 'Intro. to Digital Systems', 'Elec. Eng.', '3');
insert into course values ('FIN-201', 'Investment Banking', 'Finance', '3');
insert into course values ('HIS-351', 'World History', 'History', '3');
insert into course values ('MU-199', 'Music Video Production', 'Music', '3');
insert into course values ('PHY-101', 'Physical Principles', 'Physics', '4');
insert into instructor values ('10101', 'Srinivasan', 'Comp. Sci.', '65000');
insert into instructor values ('12121', 'Wu', 'Finance', '90000');
insert into instructor values ('15151', 'Mozart', 'Music', '40000');
insert into instructor values ('22222', 'Einstein', 'Physics', '95000');
insert into instructor values ('32343', 'El Said', 'History', '60000');
insert into instructor values ('33456', 'Gold', 'Physics', '87000');
insert into instructor values ('45565', 'Katz', 'Comp. Sci.', '75000');
insert into instructor values ('58583', 'Califieri', 'History', '62000');
insert into instructor values ('76543', 'Singh', 'Finance', '80000');
insert into instructor values ('76766', 'Crick', 'Biology', '72000');
insert into instructor values ('83821', 'Brandt', 'Comp. Sci.', '92000');
insert into instructor values ('98345', 'Kim', 'Elec. Eng.', '80000');
insert into section values ('BIO-101', '1', 'Summer', '2017', 'Painter', '514', 'B');
insert into section values ('BIO-301', '1', 'Summer', '2018', 'Painter', '514', 'A');
insert into section values ('CS-101', '1', 'Fall', '2017', 'Packard', '101', 'H');
insert into section values ('CS-101', '1', 'Spring', '2018', 'Packard', '101', 'F');
insert into section values ('CS-190', '1', 'Spring', '2017', 'Taylor', '3128', 'E');
insert into section values ('CS-190', '2', 'Spring', '2017', 'Taylor', '3128', 'A');
insert into section values ('CS-315', '1', 'Spring', '2018', 'Watson', '120', 'D');
insert into section values ('CS-319', '1', 'Spring', '2018', 'Watson', '100', 'B');
insert into section values ('CS-319', '2', 'Spring', '2018', 'Taylor', '3128', 'C');
insert into section values ('CS-347', '1', 'Fall', '2017', 'Taylor', '3128', 'A');
insert into section values ('EE-181', '1', 'Spring', '2017', 'Taylor', '3128', 'C');
insert into section values ('FIN-201', '1', 'Spring', '2018', 'Packard', '101', 'B');
insert into section values ('HIS-351', '1', 'Spring', '2018', 'Painter', '514', 'C');
insert into section values ('MU-199', '1', 'Spring', '2018', 'Packard', '101', 'D');
insert into section values ('PHY-101', '1', 'Fall', '2017', 'Watson', '100', 'A');
insert into teaches values ('10101', 'CS-101', '1', 'Fall', '2017');
insert into teaches values ('10101', 'CS-315', '1', 'Spring', '2018');
insert into teaches values ('10101', 'CS-347', '1', 'Fall', '2017');
insert into teaches values ('12121', 'FIN-201', '1', 'Spring', '2018');
insert into teaches values ('15151', 'MU-199', '1', 'Spring', '2018');
insert into teaches values ('22222', 'PHY-101', '1', 'Fall', '2017');
insert into teaches values ('32343', 'HIS-351', '1', 'Spring', '2018');
insert into teaches values ('45565', 'CS-101', '1', 'Spring', '2018');
insert into teaches values ('45565', 'CS-319', '1', 'Spring', '2018');
insert into teaches values ('76766', 'BIO-101', '1', 'Summer', '2017');
insert into teaches values ('76766', 'BIO-301', '1', 'Summer', '2018');
insert into teaches values ('83821', 'CS-190', '1', 'Spring', '2017');
insert into teaches values ('83821', 'CS-190', '2', 'Spring', '2017');
insert into teaches values ('83821', 'CS-319', '2', 'Spring', '2018');
insert into teaches values ('98345', 'EE-181', '1', 'Spring', '2017');
insert into student values ('00128', 'Zhang', 'Comp. Sci.', '102');
insert into student values ('12345', 'Shankar', 'Comp. Sci.', '32');
insert into student values ('19991', 'Brandt', 'History', '80');
insert into student values ('23121', 'Chavez', 'Finance', '110');
insert into student values ('44553', 'Peltier', 'Physics', '56');
insert into student values ('45678', 'Levy', 'Physics', '46');
insert into student values ('54321', 'Williams', 'Comp. Sci.', '54');
insert into student values ('55739', 'Sanchez', 'Music', '38');
insert into student values ('70557', 'Snow', 'Physics', '0');
insert into student values ('76543', 'Brown', 'Comp. Sci.', '58');
insert into student values ('76653', 'Aoi', 'Elec. Eng.', '60');
insert into student values ('98765', 'Bourikas', 'Elec. Eng.', '98');
insert into student values ('98988', 'Tanaka', 'Biology', '120');
insert into takes values ('00128', 'CS-101', '1', 'Fall', '2017', 'A');
insert into takes values ('00128', 'CS-347', '1', 'Fall', '2017', 'A-');
insert into takes values ('12345', 'CS-101', '1', 'Fall', '2017', 'C');
insert into takes values ('12345', 'CS-190', '2', 'Spring', '2017', 'A');
insert into takes values ('12345', 'CS-315', '1', 'Spring', '2018', 'A');
insert into takes values ('12345', 'CS-347', '1', 'Fall', '2017', 'A');
insert into takes values ('19991', 'HIS-351', '1', 'Spring', '2018', 'B');
insert into takes values ('23121', 'FIN-201', '1', 'Spring', '2018', 'C+');
insert into takes values ('44553', 'PHY-101', '1', 'Fall', '2017', 'B-');
insert into takes values ('45678', 'CS-101', '1', 'Fall', '2017', 'F');
insert into takes values ('45678', 'CS-101', '1', 'Spring', '2018', 'B+');
insert into takes values ('45678', 'CS-319', '1', 'Spring', '2018', 'B');
insert into takes values ('54321', 'CS-101', '1', 'Fall', '2017', 'A-');
insert into takes values ('54321', 'CS-190', '2', 'Spring', '2017', 'B+');
insert into takes values ('55739', 'MU-199', '1', 'Spring', '2018', 'A-');
insert into takes values ('76543', 'CS-101', '1', 'Fall', '2017', 'A');
insert into takes values ('76543', 'CS-319', '2', 'Spring', '2018', 'A');
insert into takes values ('76653', 'EE-181', '1', 'Spring', '2017', 'C');
insert into takes values ('98765', 'CS-101', '1', 'Fall', '2017', 'C-');
insert into takes values ('98765', 'CS-315', '1', 'Spring', '2018', 'B');
insert into takes values ('98988', 'BIO-101', '1', 'Summer', '2017', 'A');
insert into takes values ('98988', 'BIO-301', '1', 'Summer', '2018', null);
insert into advisor values ('00128', '45565');
insert into advisor values ('12345', '10101');
insert into advisor values ('23121', '76543');
insert into advisor values ('44553', '22222');
insert into advisor values ('45678', '22222');
insert into advisor values ('76543', '45565');
insert into advisor values ('76653', '98345');
insert into advisor values ('98765', '98345');
insert into advisor values ('98988', '76766');
insert into time_slot values ('A', 'M', '8', '0', '8', '50');
insert into time_slot values ('A', 'W', '8', '0', '8', '50');
insert into time_slot values ('A', 'F', '8', '0', '8', '50');
insert into time_slot values ('B', 'M', '9', '0', '9', '50');
insert into time_slot values ('B', 'W', '9', '0', '9', '50');
insert into time_slot values ('B', 'F', '9', '0', '9', '50');
insert into time_slot values ('C', 'M', '11', '0', '11', '50');
insert into time_slot values ('C', 'W', '11', '0', '11', '50');
insert into time_slot values ('C', 'F', '11', '0', '11', '50');
insert into time_slot values ('D', 'M', '13', '0', '13', '50');
insert into time_slot values ('D', 'W', '13', '0', '13', '50');
insert into time_slot values ('D', 'F', '13', '0', '13', '50');
insert into time_slot values ('E', 'T', '10', '30', '11', '45 ');
insert into time_slot values ('E', 'R', '10', '30', '11', '45 ');
insert into time_slot values ('F', 'T', '14', '30', '15', '45 ');
insert into time_slot values ('F', 'R', '14', '30', '15', '45 ');
insert into time_slot values ('G', 'M', '16', '0', '16', '50');
insert into time_slot values ('G', 'W', '16', '0', '16', '50');
insert into time_slot values ('G', 'F', '16', '0', '16', '50');
insert into time_slot values ('H', 'W', '10', '0', '12', '30');
insert into prereq values ('BIO-301', 'BIO-101');
insert into prereq values ('BIO-399', 'BIO-101');
insert into prereq values ('CS-190', 'CS-101');
insert into prereq values ('CS-315', 'CS-101');
insert into prereq values ('CS-319', 'CS-101');
insert into prereq values ('CS-347', 'CS-101');
insert into prereq values ('EE-181', 'PHY-101');

```

## Here's a list of SQL practice questions for you:

#### Basic SELECTs:

- Retrieve all columns from the "instructor" table.

```sql
select * from instructor;
```

- Display the unique department names from the "department" table.

```sql
select distinct dept_name from department;
```

#### Filtering and Sorting:

- Show the names and salaries of instructors in the "Comp. Sci." department.

```sql
select name ,salary from instructor where dept_name="Comp. Sci.";

```

- List the courses in the "Fall" semester of the year 2018, ordered by course title.

```sql
select distinct(course_id),title from
course natural join section
where section.semester="Spring" and year="2018" order by course.title;
```

#### Joins:

- Retrieve the names of students along with the course title and grade they obtained (if any).

```sql
SELECT student.name, course.title, takes.grade
from student left join takes on student.ID=takes.ID  join course on takes.course_id= course.course_id;
```

- Display the course titles and instructors' names for all courses.

```sql
SELECT course.title, instructor.name AS instructor_name
FROM course
JOIN teaches ON course.course_id = teaches.course_id
JOIN instructor ON teaches.ID = instructor.ID;
```

#### Aggregation:

- Calculate the average budget of all departments.

```sql
SELECT AVG(budget) AS average_budget
FROM department;
```

- Find the maximum capacity among all classrooms.

```sql
SELECT MAX(capacity) AS max_capacity
FROM classroom;
```

- Calculate the total number of credits offered by each department.

```sql
SELECT dept_name, SUM(credits) AS total_credits
FROM course
GROUP BY dept_name;
```

- Find the department with the highest total budget.

```sql
SELECT MAX(budget)
FROM department;
```

- List the average salary of instructors for each department, and only show departments with an average salary greater than 80000.

```sql
select dept_name, avg(salary) avg_sal from instructor group by dept_name having avg(salary) >80000;
```

- Calculate the total number of students in each department who have taken a course.

```sql
SELECT student.dept_name, COUNT(DISTINCT student.ID) AS total_students
FROM student
JOIN takes ON student.ID = takes.ID
GROUP BY student.dept_name;
```

- Calculate the total number of courses offered in each semester.

```sql
SELECT semester, COUNT(DISTINCT course_id) AS total_courses
FROM section
GROUP BY semester;
```

- Find the titles of courses in the Comp. Sci. department that have 3 credits. <br>

```sql
SELECT title
FROM course
WHERE dept_name = 'Comp. Sci.' AND credits = 3;
```

- Find the IDs of all students who were taught by an instructor named Einstein;
  make sure there are no duplicates in the result.
  This query can be answered in several different ways. One way is as follows.

```sql
SELECT DISTINCT takes.ID
FROM takes, instructor, teaches
WHERE takes.course_id = teaches.course_id AND
    takes.sec_id = teaches.sec_id AND
    takes.semester = teaches.semester AND
    takes.year = teaches.year AND
    teaches.id = instructor.id AND
    instructor.name = 'Einstein'
```

- Find all instructors earning the highest salary (there may be more than one with
  the same salary).

```sql
SELECT id, name
FROM instructor
WHERE salary = (SELECT MAX(salary) FROM instructor)
```

- Find the enrollment of each section that was offered in Fall 2017.

```sql
SELECT course_id, sec_id, COUNT(id)
FROM takes
WHERE semester = 'Fall' AND year = 2017
GROUP BY course_id, sec_id
```

- Find the maxium enrollment, across all sections, in Fall 2017.

```sql
SELECT course_id, sec_id, COUNT(id) as cid
FROM takes
WHERE semester = 'Fall' AND year = 2017
GROUP BY course_id, sec_id
HAVING cid = (SELECT  max(enrollment) from(sELECT COUNT(id) as enrollment
                          FROM takes
                          WHERE semester = 'Fall' AND year = 2017
                          GROUP BY course_id, sec_id) AS subquery) ;
```

- Increase the salary of each instructor in the Comp. Sci. department by 10%.

```sql
UPDATE instructor
SET salary = salary * 1.10
WHERE dept_name = 'Comp. Sci.'
```

- Delete all courses that have never been offered (i.e., do not occur in the _section_ relation).

```sql
DELETE FROM course
WHERE course_id NOT IN (SELECT course_id FROM section)
```

- finds departments whose names contain the string "sci" as a substring.

```sql
SELECT dept_name
FROM department
WHERE LOWER(dept_name) LIKE '%sci%';
```

- write an SQL query to find the name and ID of those
  Accounting students advised by an instructor in the Physics department.

```sql
SELECT s.id, s.name
FROM student AS s INNER JOIN advisor AS a ON s.id = a.s_id INNER JOIN instructor AS i ON a.i_id = i.id
WHERE s.dept_name = 'Accounting' AND i.dept_name = 'Physics';
```

- write an SQL query to find the names of those departments whose budget is higher than that of Philosophy.List them in alphabetic order.

```sql
SELECT dept_name
FROM department
WHERE budget > (SELECT budget FROM department WHERE dept_name = 'Philosophy')
ORDER BY dept_name ASC;
```

- Display a list of all instructors, showing each instructor's ID and the number of sections
  taught.

```sql
select ID,count(sec_id) from instructor natural left join teaches group by ID ;
```

- Display a list of all instructors, showing each instructor's ID and the number of sections
  taught.

```sql
SELECT ID, COUNT(sec_id) AS Number_of_sections
FROM instructor NATURAL LEFT OUTER JOIN teaches
GROUP BY ID
```

- Retrieve the details of the instructor(s) who teach the maximum number of courses, including the course details for each instructor.

```sql
select * from teaches natural join (select ID,count(id) as cid from instructor natural left join teaches group by ID having cid=(
select  max(cid) as mid from (select ID,count(id) as cid from instructor natural left join teaches group by ID ) as s1) ) as s3 ;
```

## **Here are some specific trigger-related questions:-**

#### Audit Log Trigger:

- Implement a trigger that logs changes to the "budget" column in the "department" table into an audit log table whenever a department's budget is updated.

```sql
drop table department_audit_log_on;
create table department_audit_log_on (
dept_name varchar(20),
old_budget numeric(10,2),
new_budget numeric(10,2)
);
create trigger department_budget_audit
after update on department
for each row
insert into department_audit_log_on values(OLD.dept_name,OLD.budget,NEW.budget);
drop trigger department_budget_audit;
select * from department;
select * from department_audit_log_on;
UPDATE department
SET  budget = 16000.00
WHERE dept_name = "Biology";
```

- Design a trigger that inserts a record into an audit log table whenever a course is deleted from the "course" table, capturing course details and the deletion timestamp.

```sql
CREATE TABLE course_det (
    course_id   VARCHAR(8),
    title       VARCHAR(50),
    dept_name   VARCHAR(20),
    credits     NUMERIC(2,0) CHECK (credits > 0),
    delete_time TIMESTAMP ,
    PRIMARY KEY (course_id)
);
drop table course_det;
drop trigger course_details_dlt;
create trigger course_details_dlt
before delete on course
for each row
insert into course_det values(OLD.course_id,OLD.title,OLD.dept_name,OLD.credits,CURRENT_TIMESTAMP());

delete from course where course_id ="BIO-101";
select * from course_det;
```

#### Cascade Delete Trigger:

- Design a trigger that deletes all records from the "takes" table associated with a student when that student is deleted from the "student" table.

```sql
select * from takes;
select * from student;
create trigger dlt_stu
before delete on student
for each row
delete from takes where ID=OLD.ID;
SET SQL_SAFE_UPDATES = 0;

delete from student where ID=00128;
```

#### Validation Trigger:

- Design a trigger that prevents inserting a new course into the course table if the specified credits exceed a certain limit (e.g., 6 credits).

```sql
select * from course;
DELIMITER //
create trigger course_insert
before insert on course
for each row
begin
 declare course_limit int;
 set course_limit=5;
 if new.credits > course_limit then
 signal sqlstate "45000"
 set message_text="error in credits limit";
 end if;
 end;
 //
insert into course values("BIO-678","nothing","Karma",3);
```

#### Prevent Duplicate Trigger:

- Implement a trigger to prevent the insertion of a new course with the same "course_id" in the "course" table.

```sql
DELIMITER //
CREATE TRIGGER before_insert_course
BEFORE INSERT ON course
FOR EACH ROW
BEGIN
    DECLARE course_exists INT;
    SELECT COUNT(*) INTO course_exists
    FROM course
    WHERE course_id = NEW.course_id;
    IF course_exists > 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Course with the same course_id already exists.';
    END IF;
END;
//
DELIMITER ;

```

#### Calculate Aggregates Trigger:

- Design a trigger that calculates and updates the average budget of all departments in the "dept_a" table whenever a new department is added .

```sql
create table dept_a
(avg_dept numeric(12,2),
up_time timestamp);

DELIMITER //
create trigger dept_tr
after insert on department
for each row
begin
declare tot_budget numeric(12,2);
declare tot_dept int;
select SUM(budget),COUNT(*) into tot_budget,tot_dept
from department;
insert into dept_a values(tot_budget/tot_dept,current_timestamp());
end;
//
DELIMITER ;
select * from department;
select * from dept_a;
insert into department values("Try2","Try2",30000);
drop trigger dept_tr;
select avg(budget) from department ;
```

#### Temporal Trigger:

- Create a trigger to archive or move records from the "takes" table to a history table whenever a student drops a course.

```sql
select * from takes;
delete from takes where ID=12345 and course_id ="CS-101";
CREATE TABLE takes_history (
    ID VARCHAR(5),
    course_id VARCHAR(8),
    sec_id VARCHAR(8),
    semester VARCHAR(6),
    year NUMERIC(4, 0),
    grade VARCHAR(2),
    drop_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (ID, course_id, sec_id, semester, year)
);

DELIMITER //
CREATE TRIGGER archive_dropped_course
AFTER DELETE ON takes
FOR EACH ROW
BEGIN
    INSERT INTO takes_history (ID, course_id, sec_id, semester, year, grade)
    VALUES (OLD.ID, OLD.course_id, OLD.sec_id, OLD.semester, OLD.year, OLD.grade);
END;
// DELIMITER ;
delete from takes where ID=12345 and course_id ="CS-190";
select * from takes_history;

```

- Creating a trigger that logs changes to the "student" table into a history table (student_history) whenever a student record is updated.

```sql
-- Drop the student history table if it exists
DROP TABLE IF EXISTS student_history;

-- Create the student history table
CREATE TABLE student_history (
    ID VARCHAR(5),
    name VARCHAR(20) NOT NULL,
    dept_name VARCHAR(20),
    tot_cred NUMERIC(3,0) CHECK (tot_cred >= 0),
    update_time TIMESTAMP,
    PRIMARY KEY (ID)
);

-- Create a trigger to log updates into the student history table
CREATE TRIGGER student_update
AFTER UPDATE ON student
FOR EACH ROW
INSERT INTO student_history VALUES (OLD.ID, OLD.name, OLD.dept_name, OLD.tot_cred, CURRENT_TIMESTAMP());

-- Allow unsafe updates for demonstration purposes
SET SQL_SAFE_UPDATES = 0;

-- Update a student record
UPDATE student
SET ID = '12345', name = 'minu', dept_name = 'Comp. Sci.', tot_cred = 33
WHERE ID = '12345';

-- Display the updated student table
SELECT * FROM student;

-- Display the student history table
SELECT * FROM student_history;

```

- Creating a trigger that logs the details of a deleted course into a separate table (course_det).

```sql
-- Drop the course_det table and course_details_dlt trigger if they exist
DROP TABLE IF EXISTS course_det;
DROP TRIGGER IF EXISTS course_details_dlt;

-- Create the course_det table
CREATE TABLE course_det (
    course_id   VARCHAR(8),
    title       VARCHAR(50),
    dept_name   VARCHAR(20),
    credits     NUMERIC(2,0) CHECK (credits > 0),
    delete_time TIMESTAMP,
    PRIMARY KEY (course_id)
);

-- Create a trigger to log details before deleting a course
CREATE TRIGGER course_details_dlt
BEFORE DELETE ON course
FOR EACH ROW
INSERT INTO course_det VALUES (OLD.course_id, OLD.title, OLD.dept_name, OLD.credits, CURRENT_TIMESTAMP());

-- Delete a course record
DELETE FROM course WHERE course_id = 'BIO-101';

-- Display the contents of the course_det table
SELECT * FROM course_det;

```

- Creating a trigger to insert records into a separate table (section_int) when new records are inserted into the section table.

```sql
-- Drop the section_int table and section_tr trigger if they exist
DROP TABLE IF EXISTS section_int;
DROP TRIGGER IF EXISTS section_tr;

-- Create the section_int table
CREATE TABLE section_int (
    course_id     VARCHAR(8),
    sec_id        VARCHAR(8),
    semester      VARCHAR(6) CHECK (semester IN ('Fall', 'Winter', 'Spring', 'Summer')),
    year          NUMERIC(4,0) CHECK (year > 1701 AND year < 2100),
    building      VARCHAR(15),
    room_number   VARCHAR(7),
    time_slot_id  VARCHAR(4),
    insert_time   TIMESTAMP,
    PRIMARY KEY (course_id, sec_id, semester, year)
);

-- Create a trigger to insert records into section_int after inserting into section
CREATE TRIGGER section_tr
AFTER INSERT ON section
FOR EACH ROW
INSERT INTO section_int VALUES (
    NEW.course_id,
    NEW.sec_id,
    NEW.semester,
    NEW.year,
    NEW.building,
    NEW.room_number,
    NEW.time_slot_id,
    CURRENT_TIMESTAMP()
);

-- Insert a new record into the section table
INSERT INTO section VALUES ("BIO-301", 2, "Summer", 2018, "Painter", 514, "A");

-- Display the contents of the section and section_int tables
SELECT * FROM section;
SELECT * FROM section_int;

```

- After the insertion of any new section in the section table, execute the trigger SQL queries to determine the total count of sections.

```sql
create table sec_count (total_sec int,time_count timestamp);
drop table sec_count;
select count(cid) from(select count(*) cid from section group by sec_id) as s1;

DELIMITER //
CREATE TRIGGER section_ct
AFTER INSERT ON section
FOR EACH ROW
BEGIN
    DECLARE ct INT;
    SELECT COUNT(*) AS ct INTO ct FROM (SELECT COUNT(*) AS cid FROM section GROUP BY sec_id) AS s1;
    INSERT INTO sec_count VALUES (ct, CURRENT_TIMESTAMP());
END;
//
DELIMITER ;
select * from section;
insert into section values ("CS-101",3,"Summer","2018","Painter",514,"A");
select * from sec_count;
```

### Some SQL Commands

```
SELECT - extracts data
UPDATE - updates data
DELETE - deletes data
INSERT INTO - inserts new data
CREATE DATABASE - creates a new database
ALTER DATABASE - modifies a database
CREATE TABLE - creates a new table
ALTER TABLE - modifies a table
DROP TABLE - deletes a table

```

```sql
use university;
show databases;
```

```sql

SELECT COUNT(DISTINCT id) as sid FROM student;

select * from student;

SELECT * FROM student
WHERE NOT name = 'Snow';

SELECT * FROM student
WHERE name = 'minu' AND (dept_name = 'Comp. Sci.' OR dept_name = 'Finance');

SELECT * FROM student
WHERE NOT dept_name = 'Comp. Sci.' AND NOT dept_name = 'History';

SELECT * FROM student
ORDER BY tot_cred desc;

SELECT * FROM student
ORDER BY tot_cred, id;

select * from takes;

SELECT * FROM takes
ORDER BY year ASC, grade DESC;

select * from section;

SELECT id, course_id, sec_id
FROM takes
WHERE sec_id IS not NULL;

SET SQL_SAFE_UPDATES = 0;

select * from instructor;

UPDATE instructor
SET name = 'Alfred Schmidt'
WHERE id = 10101;

DELETE FROM instructor WHERE name='wu';

SELECT * FROM instructor
LIMIT 3  OFFSET 1;

select min(salary) as minSalary from instructor;

select avg(salary) as avgSalary from instructor;

select sum(salary) as totSalary from instructor;

select * from teaches;

SELECT * FROM teaches
WHERE semester LIKE '_p%';

SELECT * FROM teaches
WHERE semester not LIKE '_p%';

-- select all where semester has at least 3 charecter.
SELECT * FROM teaches
WHERE semester LIKE 's_%';

SELECT * FROM teaches
WHERE semester LIKE 's%g';

SELECT * FROM teaches
WHERE semester LIKE 's_r__g';

SELECT * FROM teaches
WHERE semester NOT IN ('Spring',"Fall");

SELECT * FROM teaches
WHERE sec_id IN (SELECT sec_id FROM teaches);

SELECT * FROM teaches
WHERE id NOT BETWEEN 10 AND 10101;


SELECT *
FROM (instructor i
INNER JOIN teaches t ON t.ID = i.id);

SELECT *
FROM (instructor i
left JOIN teaches t ON t.ID = i.id);

SELECT *
FROM (instructor i
right JOIN teaches t ON t.ID = i.id) order by i.id;

SELECT *
FROM instructor
cross JOIN teaches;

--structure of commands
SELECT
FROM
GROUP BY
HAVING
ORDER BY  DESC;
```

```
DATE - format YYYY-MM-DD
DATETIME - format: YYYY-MM-DD HH:MI:SS
TIMESTAMP - format: YYYY-MM-DD HH:MI:SS
```

```sql
SELECT sec_id, course_id, COUNT(*) AS occurrence
FROM section
GROUP BY sec_id, course_id;

SELECT sec_id, course_id, MAX(occurrence) AS max_occurrence
FROM (
    SELECT sec_id, course_id, COUNT(*) AS occurrence
    FROM section
    GROUP BY sec_id, course_id
) AS t1
GROUP BY sec_id, course_id;



create view test as
select id,name from instructor;

select * from test;

create or replace view test as
select id,name, dept_name from instructor;

drop view test;
```

```sql
SELECT s.id
FROM student s LEFT OUTER JOIN advisor a
    ON s.id = a.s_id
WHERE a.i_id IS NULL
    OR a.s_id IS NULL;
```

```sql
 SELECT year, SUM(credits)
    FROM takes NATURAL JOIN course
    GROUP BY year
    ORDER BY year ASC;
```

- Find the ID of each student who has never taken a course at the university.

```sql
SELECT s.ID
FROM student s LEFT OUTER JOIN takes t
    ON s.ID = t.ID
WHERE t.ID IS NULL;
```

- Display a list of all instructors, showing each instructor's ID and the number of sections
  taught.

```sql
SELECT ID, COUNT(sec_id) AS Number_of_sections
FROM instructor NATURAL LEFT OUTER JOIN teaches
GROUP BY ID
```

```sql
SELECT course_id, sec_id, year, semester, COUNT(DISTINCT ID) AS num
FROM takes
GROUP BY course_id, sec_id, year, semester;
```

- write an SQL query to find the ID and title of each course in Comp. Sci. that has had at least one section with afternoon hours

```sql
SELECT course_id,title
FROM course AS c
WHERE dept_name = 'Comp. Sci.' AND
    EXISTS (
        SELECT *
        FROM section
        WHERE section.course_id = c.course_id AND
        time_slot_id IN (SELECT time_slot_id FROM time_slot WHERE end_hr >= 12)
    )
```

- write an SQL query to find the ID and name of each instructor who has never given an A grade in any course she or he has taught and who has given at least one other non-null grade in some course.

```sql
SELECT id, name
FROM instructor AS i
WHERE 'A' NOT IN (
    SELECT takes.grade
    FROM takes INNER JOIN teaches
        ON (takes.course_id,takes.sec_id,takes.semester,takes.year) =
           (teaches.course_id,teaches.sec_id,teaches.semester,teaches.year)
    WHERE teaches.id = i.id
)
AND
(
    SELECT COUNT(*)
    FROM takes INNER JOIN teaches
        ON (takes.course_id,takes.sec_id,takes.semester,takes.year) =
           (teaches.course_id,teaches.sec_id,teaches.semester,teaches.year)
    WHERE teaches.id = i.id AND takes.grade IS NOT NULL
) >= 1
```

> there are other possible instances of that relation for which the result would _not_ be zero. Give one such instance, and explain why the result would not be zero.

```sql
SELECT AVG(salary) - (SUM(salary)/COUNT(*))
FROM instructor
```

> All aggregate functions except **count(\*)** ignore null values in their input collection.

- write an SQL query to find the name and ID of each History student whose name begins with the letter 'D' and who has _not_ taken at least five Music courses.

```sql
SELECT id,name
FROM student AS s
WHERE dept_name = 'History'
    AND name LIKE 'D%'
    AND (
        SELECT COUNT(DISTINCT course_id)
        FROM takes
        WHERE takes.id = s.id AND
            course_id IN (SELECT course_id FROM course WHERE dept_name = 'Music')
    ) < 5;
```
