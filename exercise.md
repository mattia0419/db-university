1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT *
 FROM `students` 
 WHERE `date_of_birth` LIKE "1990-%"; 

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

SELECT * 
FROM `courses` 
WHERE `cfu` > 10;

3. Selezionare tutti gli studenti che hanno più di 30 anni

SELECT * FROM `students` WHERE `date_of_birth` < DATE_SUB(CURDATE(), INTERVAL 30 YEAR);

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286)

SELECT * FROM `courses` WHERE `year` = "1" AND `period` = "I semestre";

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)

SELECT * 
FROM `exams` 
WHERE `date` LIKE "2020-06-20" AND `hour` BETWEEN "14:00:00" AND "19:00:00";

6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT * 
FROM `degrees` 
WHERE `level` LIKE "magistrale";

7. Da quanti dipartimenti è composta l'università? (12)

SELECT COUNT(*) 
FROM `departments`;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

SELECT COUNT(*) 
FROM `teachers` 
WHERE `phone` IS NULL;

GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno

SELECT YEAR(`enrolment_date`), COUNT(`id`) FROM `students` GROUP BY YEAR(`enrolment_date`);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT `office_address`, COUNT(`id`) FROM `teachers` GROUP BY `office_address`;

3. Calcolare la media dei voti di ogni appello d'esame

SELECT `exam_id`, AVG(`vote`) FROM `exam_student` GROUP BY `exam_id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT `department_id`, COUNT(`name`) FROM `degrees` GROUP BY `department_id`;

JOIN

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT `students`.*, `degrees`.`name` "corso di laurea" FROM `students` INNER JOIN `degrees` ON `degrees`.`name`="Corso di Laurea in Economia";

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze

SELECT `departments`.`name` "nome_dipartimento", `degrees`.`name` "nome_corso_di_laurea" FROM `departments` INNER JOIN `degrees` ON `level`="magistrale" WHERE `departments`.`name`="Dipartimento di Neuroscienze";

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT `teachers`.`name`, `teachers`.`surname`, `courses`.`name` FROM `course_teacher` INNER JOIN `teachers` ON `teachers`.`id`=`course_teacher`.`teacher_id` INNER JOIN `courses` ON `courses`.`id`=`course_teacher`.`course_id` WHERE `teachers`.`id`= 44;

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome

SELECT `students`.*, `degrees`.`name` "nome_corso_di_laurea", `departments`.`name` "nome_dipartimento" FROM `students` INNER JOIN `degrees` ON `students`.`degree_id`=`degrees`.`id` INNER JOIN `departments` ON `degrees`.`department_id`=`departments`.`id` ORDER BY `students`.`surname` ASC, `students`.`name` ASC;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT `teachers`.`name`, `teachers`.`surname`, `degrees`.`name` "nome_corso_di_laurea", `courses`.`name` "nome_corso" FROM `course_teacher` INNER JOIN `courses` ON `course_teacher`.`course_id`=`courses`.`id` INNER JOIN `teachers` ON `course_teacher`.`teacher_id`=`teachers`.`id` INNER JOIN `degrees` ON `courses`.`degree_id`=`degrees`.`id`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)

SELECT DISTINCT `teachers`.`name`, `teachers`.`surname`, `departments`.`name` "nome_dipartimento" FROM `course_teacher` INNER JOIN `courses` ON `course_teacher`.`course_id`=`courses`.`id` INNER JOIN `teachers` ON `course_teacher`.`teacher_id`=`teachers`.`id` INNER JOIN `degrees` ON `courses`.`degree_id`=`degrees`.`id` INNER JOIN `departments` ON `degrees`.`department_id`=`departments`.`id` WHERE `departments`.`name`="Dipartimento di Matematica";

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.
