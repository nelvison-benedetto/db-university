## SQL QUERY

SELECT * from course_teacher;
SELECT * from courses;
SELECT * from degrees;
SELECT * from departments;
SELECT * from exam_student;
SELECT * from exams;
SELECT * from students;
SELECT * from teachers;

## sql 04/12/2024

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

## GROUPS BY

SELECT YEAR(enrolment_date),COUNT(*)
FROM students 
GROUP BY YEAR(enrolment_date);

SELECT office_address, COUNT(*)
from teachers
GROUP BY office_address;

SELECT exam_id, AVG(vote)
FROM exam_student
GROUP BY exam_id;

SELECT department_id, COUNT(*)
FROM degrees
GROUP BY department_id;

## JOINS

- Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sql
SELECT students.*
FROM degrees INNER JOIN students
ON degrees.id = students.degree_id
WHERE degrees.name = 'Corso di Laurea in Economia';
```

- Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT degrees.*
FROM departments INNER JOIN degrees
ON departments.id = degrees.department_id
WHERE (departments.name='Dipartimento di Neuroscienze' AND degrees.name LIKE 'Corso di Laurea Magistrale%');
```

- Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT courses.*
FROM teachers INNER JOIN course_teacher
ON teachers.id = course_teacher.teacher_id
INNER JOIN courses
ON course_teacher.course_id = courses.id
WHERE (teachers.name='Fulvio' AND teachers.surname='Amato');
```

- Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT students.*, 
	degrees.name AS degree_name,
    departments.name AS department_name
FROM students INNER JOIN degrees
ON students.degree_id = degrees.id
INNER JOIN departments
ON degrees.department_id = departments.id
ORDER BY students.surname, students.name;
```

- Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sql
SELECT degrees.id AS degree_id, 
	courses.degree_id AS course_degree_id,
	courses.name AS course_name, 
    teachers.name AS teacher_name,
    teachers.surname AS teacher_surname
FROM degrees INNER JOIN courses
ON degrees.id = courses.degree_id
INNER JOIN course_teacher
ON courses.id = course_teacher.course_id
INNER JOIN teachers
ON course_teacher.teacher_id = teachers.id
ORDER BY courses.degree_id;
```

- Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sql
SELECT SUM(result_table2.num_copies)
FROM
(
	SELECT COUNT(result_table1.count_teacher_id) AS num_copies
	FROM  
	(
		SELECT departments.name, COUNT(teachers.id) AS count_teacher_id, teachers.name AS teacher_name, teachers.surname AS teacher_surname
		FROM departments INNER JOIN degrees
		ON departments.id = degrees.department_id
		INNER JOIN courses 
		ON degrees.id = courses.degree_id
		INNER JOIN course_teacher
		ON courses.id = course_teacher.course_id
		INNER JOIN teachers
		ON course_teacher.teacher_id = teachers.id
		WHERE departments.name = 'Dipartimento di Matematica'
		GROUP BY (teachers.id)
	)
	AS result_table1
	GROUP BY (count_teacher_id)
) 
AS result_table2;
```