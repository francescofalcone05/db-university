GROUP BY
1. Contare quanti iscritti ci sono stati ogni anno
SELECT COUNT(id) AS nr_studenti, year( enrolment_date) AS anno FROM `students` GROUP BY year( enrolment_date);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT COUNT(id) AS nr_insegnanti, office_address AS ufficio FROM `teachers` GROUP BY office_address;

3. Calcolare la media dei voti di ogni appello d'esame
SELECT exam_id AS esame, AVG(vote) AS media_voti FROM `exam_student` GROUP BY exam_id;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT department_id, COUNT(id) AS nr_corsi FROM `degrees` GROUP BY department_id;


JOIN ON
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT students.name, students.surname, degrees.name FROM `students` INNER JOIN degrees ON students.degree_id = degrees.id WHERE degrees.name = 'Corso di Laurea in Economia';

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
SELECT * FROM `degrees` INNER JOIN departments ON departments.id = degrees.department_id WHERE degrees.name LIKE '%Magistrale%' AND departments.name = 'Dipartimento di Neuroscienze';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT courses.name, course_teacher.teacher_id FROM `courses` INNER JOIN course_teacher ON courses.id = course_teacher.course_id WHERE teacher_id = 44;

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
SELECT students.name, students.surname, degrees.name, departments.name FROM `students` INNER JOIN degrees ON students.degree_id = degrees.id INNER JOIN departments ON departments.id = degrees.department_id ORDER BY students.name ASC, students.surname ASC;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT courses.name, degrees.name, teachers.name, teachers.surname FROM `courses` INNER JOIN degrees ON courses.degree_id = degrees.id INNER JOIN course_teacher ON courses.id = course_teacher.course_id INNER JOIN teachers ON course_teacher.teacher_id = teachers.id ORDER BY teachers.name ASC, teachers.surname ASC;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
SELECT DISTINCT teachers.id, teachers.name, teachers.surname, departments.name FROM `course_teacher` INNER JOIN teachers ON course_teacher.teacher_id = teachers.id INNER JOIN courses ON course_teacher.course_id = courses.id INNER JOIN degrees ON courses.degree_id = degrees.id INNER JOIN departments ON degrees.department_id = departments.id WHERE departments.name LIKE '%matematica%';

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. 
Successivamente, filtrare i tentativi con voto minimo 18