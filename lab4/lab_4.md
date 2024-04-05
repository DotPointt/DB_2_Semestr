# Лабораторная работа 4
## BTree индексы, разновидности. Оптимизация. Использование
### 1.	Составьте выражения реляционной алгебры и соответствующие SQL-запросы
### Вариант 4

1.
SELECT StudentId, StudentName, GroupId
FROM Students
WHERE StudentName = :StudentName;

2.
SELECT Students.StudentId, Students.StudentName, Groups.GroupName
FROM Students
INNER JOIN Groups ON Students.GroupId = Groups.GroupId
WHERE Students.StudentId = :StudentId;

3.
SELECT Students.StudentId, Students.StudentName, Students.GroupId
FROM Students
INNER JOIN Marks ON Students.StudentId = Marks.StudentId
INNER JOIN Plan ON Marks.CourseId = Plan.CourseId
INNER JOIN Lecturers ON Plan.LecturerId = Lecturers.LecturerId
WHERE Marks.Mark = :Mark
AND Lecturers.LecturerName = :LecturerName;

4.
SELECT Students.StudentId, Students.StudentName, Students.GroupId
FROM Students
WHERE Students.StudentId NOT IN (SELECT StudentId FROM Marks WHERE CourseId = (SELECT CourseId FROM Courses WHERE CourseName = :CourseName));

5.
SELECT Students.StudentName, Courses.CourseName
FROM Students
INNER JOIN Plan ON Students.GroupId = Plan.GroupId
INNER JOIN Courses ON Plan.CourseId = Courses.CourseId
ORDER BY Students.StudentName, Courses.CourseName;

6.
SELECT Marks.StudentId
FROM Marks
INNER JOIN Plan ON Marks.CourseId = Plan.CourseId
WHERE Plan.LecturerId = (SELECT LecturerId FROM Lecturers WHERE LecturerName = :LecturerName)
GROUP BY Marks.StudentId
HAVING COUNT(DISTINCT Marks.CourseId) = (SELECT COUNT(*) FROM Plan WHERE LecturerId = (SELECT LecturerId FROM Lecturers WHERE LecturerName = :LecturerName));

7.
SELECT Groups.GroupName, Courses.CourseName
FROM Groups
CROSS JOIN Courses
WHERE NOT EXISTS (
    SELECT *
    FROM Students
    WHERE Students.GroupId = Groups.GroupId
    AND NOT EXISTS (
        SELECT *
        FROM Marks
        WHERE Marks.StudentId = Students.StudentId
        AND Marks.CourseId = Courses.CourseId
    )
);

8.
SELECT SUM(Mark) AS SumMark
FROM Marks
WHERE StudentId = :StudentId;

9.
SELECT Students.StudentName, AVG(Marks.Mark) AS AvgMark
FROM Students
LEFT JOIN Marks ON Students.StudentId = Marks.StudentId
GROUP BY Students.StudentName;

10.
SELECT StudentId, 
       COUNT(DISTINCT CourseId) AS Total,
       COUNT(CASE WHEN Mark IS NOT NULL THEN 1 END) AS Passed,
       COUNT(CASE WHEN Mark IS NULL THEN 1 END) AS Failed
FROM Marks
GROUP BY StudentId;



## Каждый запрос, переписанный в реляционную алгебру, находится в прикрепленном .docx файле.



