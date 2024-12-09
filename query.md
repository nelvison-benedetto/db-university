##SQL QUERY

SELECT * from course_teacher;
SELECT * from courses;
SELECT * from degrees;
SELECT * from departments;
SELECT * from exam_student;
SELECT * from exams;
SELECT * from students;
SELECT * from teachers;

##sql 04/12/2024

SELECT * 
FROM students
WHERE date_of_birth >=  '1990-01-01' and date_of_birth< '1991-01-01';
SELECT COUNT(*)
FROM students
WHERE YEAR(date_of_birth) = '1990';

SELECT COUNT(*) 
FROM courses 
WHERE cfu > 10;

SELECT COUNT(*) 
FROM students
WHERE TIMESTAMPDIFF (YEAR, date_of_birth, CURDATE()) > 30;

SELECT COUNT(*) 
FROM courses
WHERE (period = 'I semestre' AND year=1);

SELECT COUNT(*)
FROM exams
WHERE (date = '2020-06-20' AND hour > '14:00:00');

SELECT COUNT(*)
FROM degrees
WHERE name LIKE 'Corso di Laurea Magistrale%';

SELECT DISTINCT COUNT(*)
FROM departments;

SELECT COUNT(*)
FROM teachers
WHERE phone IS NULL;

INSERT INTO students (id, degree_id, name,surname,date_of_birth,fiscal_code,enrolment_date,registration_number,email)
VALUES('33923','61','Nelvison','Benedetto','2000-06-15', 66666, CURDATE(),66678876,'nelvis77@gmail.xom');

SELECT *
FROM students
WHERE name='Nelvison';

SELECT * 
FROM teachers
WHERE (name='Pietro' AND surname='Rizzo');

UPDATE teachers 
SET office_number = 126
WHERE (id=58 AND name='Pietro' AND surname='Rizzo');

DELETE FROM students WHERE (id=33923 AND name='Nelvison' AND surname='Benedetto');

