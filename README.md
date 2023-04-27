# DB University

## 21/04
1. Selezionare tutti gli studenti nati nel 1990 (160)
    - SELECT * FROM `students` WHERE `date_of_birth` LIKE '1990%'; 

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
    - SELECT * FROM `courses` WHERE `cfu` > '10'; 

3. Selezionare tutti gli studenti che hanno più di 30 anni
    - SELECT * FROM `students` WHERE `date_of_birth` <= '1993-04-21'; 

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
    - SELECT * FROM `courses` WHERE `period` = 'I semestre' AND `year` = '1'; 

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
    - SELECT * FROM `exams` WHERE `date` = '2020-06-20' AND `hour` > '14:00:00'; 

6. Selezionare tutti i corsi di laurea magistrale (38)
    - SELECT * FROM `degrees` WHERE `level` = 'magistrale'; 

7. Da quanti dipartimenti è composta l'università? (12)
    - SELECT COUNT(`id`) AS `contatore_dipartimenti` FROM `departments`; 

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
    - SELECT * FROM `teachers` WHERE `phone` IS NULL; 

## 26/04
## GROUP BY
1. Contare quanti iscritti ci sono stati ogni anno
    - SELECT YEAR(`students`.`enrolment_date`) AS `anno d'iscrizione`, COUNT(`id`) AS `iscritti` FROM `students` GROUP BY `anno d'iscrizione`; 

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
    - SELECT `office_number`, COUNT(id) FROM `teachers` GROUP BY `office_number`; 

3. Calcolare la media dei voti di ogni appello d'esame
    - SELECT ROUND(AVG(`vote`)), `exam_id` FROM `exam_student` GROUP BY `exam_id`; 

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
    - SELECT `department_id` AS 'dipartimento', COUNT(id) AS 'corsi di laurea' FROM `degrees` GROUP BY `department_id`; 

## JOIN
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
    - SELECT * FROM `students` JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id` WHERE `degrees`.`name` = 'corso di laurea in economia';

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
    - SELECT * FROM `departments` JOIN `degrees` ON `degrees`.`department_id` = `departments`.`id` WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name` = 'dipartimento di neuroscienze'; 

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
    - SELECT * FROM `teachers` JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id` WHERE `teachers`.`id` = 44; 

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
    - SELECT `students`.`name`, `students`.`surname`, `degrees`.`name`, `departments`.`name` FROM `students` JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id` JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` ORDER BY `students`.`surname`, `students`.`name`; 

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
    - SELECT `degrees`.`name`, `courses`.`name`, CONCAT(`teachers`.`name`, ' ', `teachers`.`surname`) AS `teacher_name` FROM `degrees` JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id` JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id` JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`; 

7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami
    - SELECT `students`.`id`, `students`.`name`, `students`.`surname`, MAX(`exam_student`.`vote`) AS `voto_esame`, MIN(`exam_student`.`vote`) AS `voto_esame_minimo`, `courses`.`name` AS `nome_corso`, COUNT(`exam_student`.`vote`) AS `contatore_esami` FROM `students` JOIN `exam_student` ON `students`.`id` = `exam_student`.`student_id` JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id` JOIN `courses` ON `exams`.`course_id` = `courses`.`id` GROUP BY `students`.`id`, `courses`.`id` HAVING `voto_esame` > 18; 