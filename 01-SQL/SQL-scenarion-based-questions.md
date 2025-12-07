# SQL Query Examples — Student, Program, Scholarship Tables

This README contains 45 example SQL queries using the Student, Program, and Scholarship tables. Each example includes the SQL query, the expected output (as provided), and a short explanation.

---

### 1. Fetch FIRST_NAME in upper case and use ALIAS name as STUDENT_NAME  
Query:
```sql
SELECT upper(FIRST_NAME) as STUDENT_NAME from Student;
```
Output:
```
STUDENT_NAME
SHIVANSH
UMESH
RAKESH
RADHA
KUSH
PREM
PANKAJ
NAVLEEN
```
Explanation:  
This query uses the UPPER() function to convert the FIRST_NAME values to uppercase. The alias STUDENT_NAME is applied to the output column for better readability and relevance.

---

### 2. Fetch unique values of MAJOR Subjects from Student table  
Query:
```sql
SELECT DISTINCT MAJOR from STUDENT; 
-- or
SELECT MAJOR FROM STUDENT GROUP BY(MAJOR);
```
Output:
```
Computer Science

Mathematics
Biology
Chemistry
Physics
History
English
```
Explanation:  
The DISTINCT keyword ensures that only unique values from the MAJOR column are retrieved, removing duplicates and providing a clean list of all subjects.

---

### 3. Print the first 3 characters of FIRST_NAME from Student table  
Query:
```sql
SELECT SUBSTRING(FIRST_NAME, 1, 3) FROM Student;
```
Output:
```
SUBSTRING(FIRST_NAME, 1, 3)
Shi
Ume
Rak
Rad
Kus
Pre
Pan
Nav
```
Explanation:  
The SUBSTRING() function extracts a portion of the string. Here, the function starts at position 1 and retrieves the next 3 characters from FIRST_NAME.

---

### 4. Find the position of alphabet ('a') in the first name 'Shivansh'  
Query:
```sql
SELECT INSTR(FIRST_NAME, 'a') FROM Student WHERE FIRST_NAME = 'Shivansh'; 
```
Output:
```
INSTR(FIRST_NAME, 'a')
5
```
Explanation:  
The INSTR() function finds the position of the first occurrence of the specified character ('a') in the string. For the name 'Shivansh', 'a' is at the 5th position.

---

### 5. Fetch the unique values of MAJOR Subjects and print its length  
Query:
```sql
SELECT MAJOR, LENGTH(MAJOR) FROM Student GROUP BY(MAJOR);                                                         
-- or                                                                                                                                                                                                                 
SELECT DISTINCT MAJOR, LENGTH(MAJOR) FROM Student;  
```

---

### 6. Print FIRST_NAME after replacing 'a' with 'A'  
Query:
```sql
SELECT REPLACE(FIRST_NAME, 'a', 'A') FROM Student;
```
Output:
```
REPLACE(FIRST_NAME, 'a', 'A')
ShivAnsh
Umesh
RAkesh
RAdhA
Kush
Prem
PAnkAj
NAvleen
```
Explanation:  
The REPLACE() function substitutes every occurrence of the letter 'a' in FIRST_NAME with 'A'. Only names containing 'a' show a difference in output.

---

### 7. Print FIRST_NAME and LAST_NAME into single column COMPLETE_NAME  
Query:
```sql
SELECT CONCAT(FIRST_NAME, ' ', LAST_NAME) AS COMPLETE_NAME FROM Student;
```
Output:
```
COMPLETE_NAME
Shivansh Mahajan
Umesh Sharma
Rakesh Kumar
Radha Sharma
Kush Kumar
Prem Chopra
Pankaj Vats
Navleen Kaur
```
Explanation:  
The CONCAT() function combines FIRST_NAME and LAST_NAME, separated by a space, into a single column named COMPLETE_NAME.

---

### 8. Print all Student details ordered by FIRST_NAME Ascending and MAJOR descending  
Query:
```sql
SELECT * FROM Student ORDER BY FIRST_NAME , MAJOR DESC;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
205	Kush	Kumar	7.85	2021-09-01 08:30:00	Physics
208	Navleen	Kaur	7	2021-09-01 06:30:00	Mathematics
207	Pankaj	Vats	9.78	2021-09-01 02:30:00	English
206	Prem	Chopra	9.56	2021-09-01 09:24:00	History
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
203	Rakesh	Kumar	5.6	2021-09-01 10:00:00	Biology
201	Shivansh	Mahajan	8.79	2021-09-01 09:30:00	Computer Science
202	Umesh	Sharma	8.44	2021-09-01 08:30:00	Mathematics
```
Explanation:  
The ORDER BY clause sorts records first by FIRST_NAME in ascending order, then by MAJOR in descending order.

---

### 9. Print details of Students with FIRST_NAME as 'Prem' and 'Shivansh'  
Query:
```sql
SELECT * from Student WHERE FIRST_NAME IN ('Prem' , 'Shivansh');
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
201	Shivansh	Mahajan	8.79	2021-09-01 09:30:00	Computer Science
206	Prem	Chopra	9.56	2021-09-01 09:24:00	History
```
Explanation:  
The IN operator checks if FIRST_NAME matches 'Prem' or 'Shivansh' and retrieves only the matching rows.

---

### 10. Print details of Students excluding FIRST_NAME 'Prem' and 'Shivansh'  
Query:
```sql
SELECT * from Student WHERE FIRST_NAME NOT IN ('Prem', 'Shivansh');
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
202	Umesh	Sharma	8.44	2021-09-01 08:30:00	Mathematics
203	Rakesh	Kumar	5.6	2021-09-01 10:00:00	Biology
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
205	Kush	Kumar	7.85	2021-09-01 08:30:00	Physics
207	Pankaj	Vats	9.78	2021-09-01 02:30:00	English
208	Navleen	Kaur	7	2021-09-01 06:30:00	Mathematics
```
Explanation:  
The NOT IN operator filters out rows where FIRST_NAME is 'Prem' or 'Shivansh'.

---

### 11. Print details of Students whose FIRST_NAME ends with 'a'  
Query:
```sql
SELECT * FROM Student WHERE FIRST_NAME LIKE '%a';
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
```
Explanation:  
The LIKE operator with %a matches any FIRST_NAME ending with the letter 'a'. % is a wildcard representing any number of characters preceding 'a'.

---

### 12. Print Students whose FIRST_NAME ends with 'a' and contains five alphabets  
Query:
```sql
SELECT * FROM Student WHERE FIRST_NAME LIKE '_____a';
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
```
Explanation:  
The LIKE operator with _____a ensures that the FIRST_NAME is exactly five characters long (denoted by four underscores) and ends with 'a'.

---

### 13. Students whose GPA lies between 9.00 and 9.99  
Query:
```sql
SELECT * FROM Student WHERE GPA BETWEEN 9.00 AND 9.99;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
206	Prem	Chopra	9.56	2021-09-01 09:24:00	History
207	Pankaj	Vats	9.78	2021-09-01 02:30:00	English
```
Explanation:  
The BETWEEN operator retrieves rows where the GPA falls within the inclusive range of 9.00 to 9.99.

---

### 14. Fetch count of Students having Major 'Computer Science'  
Query:
```sql
SELECT Major, COUNT(*) as TOTAL_COUNT FROM Student WHERE MAJOR = 'Computer Science';
```
Output:
```
MAJOR	TOTAL_COUNT
Computer Science	1
```
Explanation:  
The COUNT(*) function calculates the total number of rows where the MAJOR is 'Computer Science'.

---

### 15. Fetch Students full names with GPA between 8.5 and 9.5  
Query:
```sql
SELECT CONCAT(FIRST_NAME, ' ', LAST_NAME) AS FULL_NAME FROM Student WHERE GPA BETWEEN 8.5 and 9.5;
```
Output:
```
FULL_NAME
Shivansh Mahajan
Radha Sharma
```
Explanation:  
This query uses CONCAT() to merge FIRST_NAME and LAST_NAME into a full name and filters by GPA using BETWEEN.

---

### 16. Number of Students for each MAJOR in descending order  
Query:
```sql
SELECT MAJOR, COUNT(MAJOR) from Student group by MAJOR order by COUNT(MAJOR) DESC;
```
Output:
```
MAJOR	COUNT(MAJOR)
Mathematics	2
Physics	1
History	1
English	1
Computer Science	1
Chemistry	1
Biology	1
```
Explanation:  
GROUP BY groups rows by MAJOR, COUNT(*) computes the number of students in each group, and ORDER BY sorts descending by the count.

---

### 17. Details of students who have received scholarships (name, amount, date)  
Query:
```sql
SELECT 
    Student.FIRST_NAME,
    Student.LAST_NAME,
    Scholarship.SCHOLARSHIP_AMOUNT,
    Scholarship.SCHOLARSHIP_DATE
FROM 
    Student
INNER JOIN 
    Scholarship ON Student.STUDENT_ID = Scholarship.STUDENT_REF_ID;
```
Output:
```
FIRST_NAME	LAST_NAME	SCHOLARSHIP_AMOUNT	SCHOLARSHIP_DATE
Shivansh	Mahajan	5000	2021-10-15 00:00:00
Umesh	Sharma	4500	2022-08-18 00:00:00
Rakesh	Kumar	3000	2022-01-25 00:00:00
Shivansh	Mahajan	4000	2021-10-15 00:00:00
```
Explanation:  
INNER JOIN combines data from Student and Scholarship where STUDENT_ID matches STUDENT_REF_ID, retrieving only students who received scholarships.

---

### 18. Show only odd rows from Student table  
Query:
```sql
SELECT * FROM Student WHERE student_id % 2 != 0;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
201	Shivansh	Mahajan	8.79	2021-09-01 09:30:00	Computer Science
203	Rakesh	Kumar	5.6	2021-09-01 10:00:00	Biology
205	Kush	Kumar	7.85	2021-09-01 08:30:00	Physics
207	Pankaj	Vats	9.78	2021-09-01 02:30:00	English
```
Explanation:  
The condition STUDENT_ID % 2 != 0 filters rows where STUDENT_ID is odd.

---

### 19. Show only even rows from Student table  
Query:
```sql
SELECT * FROM Student WHERE student_id % 2 = 0;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
202	Umesh	Sharma	8.44	2021-09-01 08:30:00	Mathematics
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
206	Prem	Chopra	9.56	2021-09-01 09:24:00	History
208	Navleen	Kaur	7	2021-09-01 06:30:00	Mathematics
```
Explanation:  
The condition STUDENT_ID % 2 = 0 retrieves rows where STUDENT_ID is even.

---

### 20. List all students and their scholarship amounts if any (NULL if none)  
Query:
```sql
SELECT 
    Student.FIRST_NAME,
    Student.LAST_NAME,
    Scholarship.SCHOLARSHIP_AMOUNT,
    Scholarship.SCHOLARSHIP_DATE
FROM 
    Student
LEFT JOIN 
    Scholarship ON Student.STUDENT_ID = Scholarship.STUDENT_REF_ID;
```
Output:
```
FIRST_NAME	LAST_NAME	SCHOLARSHIP_AMOUNT	SCHOLARSHIP_DATE
Shivansh	Mahajan	5000	2021-10-15 00:00:00
Umesh	Sharma	4500	2022-08-18 00:00:00
Rakesh	Kumar	3000	2022-01-25 00:00:00
Radha	Sharma	4000	2021-10-15 00:00:00
Kush	Kumar		
Prem	Chopra		
Pankaj	Vats		
Navleen	Kaur		
```
Explanation:  
LEFT JOIN retrieves all students; scholarship fields are NULL for students without scholarship records.

---

### 21. Show the top n (5) records ordered by descending GPA  
Query:
```sql
SELECT * from Student ORDER BY GPA DESC LIMIT 5;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
207	Pankaj	Vats	9.78	2021-09-01 02:30:00	English
206	Prem	Chopra	9.56	2021-09-01 09:24:00	History
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
201	Shivansh	Mahajan	8.79	2021-09-01 09:30:00	Computer Science
202	Umesh	Sharma	8.44	2021-09-01 08:30:00	Mathematics
```
Explanation:  
ORDER BY GPA DESC sorts by GPA descending and LIMIT 5 returns the top 5 students.

---

### 22. Determine the nth (n=5) highest GPA from a table  
Query:
```sql
SELECT * FROM Student ORDER BY GPA DESC LIMIT 5, 1;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
205	Kush	Kumar	7.85	2021-09-01 08:30:00	Physics
```
Explanation:  
This query skips the top 5 records and fetches the 6th record (LIMIT 5, 1), which corresponds to the 6th-highest GPA after ordering by GPA DESC.

---

### 23. Determine the 5th highest GPA without using LIMIT  
Query:
```sql
SELECT * FROM Student s1 
WHERE 4 = (
    SELECT COUNT(DISTINCT (s2.GPA)) 
    FROM Student s2
    WHERE s2.GPA >= s1.GPA
);
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
201	Shivansh	Mahajan	8.79	2021-09-01 09:30:00	Computer Science
```
Explanation:  
A correlated subquery counts distinct GPAs greater than or equal to each student's GPA; when that count equals 4, s1 corresponds to the 5th-highest GPA.

---

### 24. Fetch the list of Students with the same GPA (self-join)  
Query:
```sql
SELECT s1.* FROM Student s1, Student s2 WHERE s1.GPA = s2.GPA AND s1.Student_id != s2.Student_id;
```
Output:
```
SQL query successfully executed. However, the result set is empty.
```
Explanation:  
The self-join identifies students sharing the same GPA. In this dataset, no two distinct students share the same GPA.

---

### 25. Show the second highest GPA using sub-query  
Query:
```sql
SELECT MAX(GPA) FROM Student
WHERE GPA NOT IN(SELECT MAX(GPA) FROM Student);
```
Output:
```
MAX(GPA)
9.56
```
Explanation:  
The subquery fetches the highest GPA; the main query excludes that value to find the second highest GPA.

---

### 26. Show one row twice in results from a table  
Query:
```sql
SELECT * FROM Student 
UNION ALL
SELECT * FROM Student ORDER BY STUDENT_ID;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
201	Shivansh	Mahajan	8.79	2021-09-01 09:30	Computer Science
201	Shivansh	Mahajan	8.79	2021-09-01 09:30	Computer Science
202	Umesh	Sharma	8.44	2021-09-01 08:30	Mathematics
202	Umesh	Sharma	8.44	2021-09-01 08:30	Mathematics
203	Rakesh	Kumar	5.6	2021-09-01 10:00	Biology
203	Rakesh	Kumar	5.6	2021-09-01 10:00	Biology
204	Radha	Sharma	9.2	2021-09-01 12:45	Chemistry
204	Radha	Sharma	9.2	2021-09-01 12:45	Chemistry
205	Kush	Kumar	7.85	2021-09-01 08:30	Physics
205	Kush	Kumar	7.85	2021-09-01 08:30	Physics
206	Prem	Chopra	9.56	2021-09-01 09:24	History
206	Prem	Chopra	9.56	2021-09-01 09:24	History
207	Pankaj	Vats	9.78	2021-09-01 02:30	English
207	Pankaj	Vats	9.78	2021-09-01 02:30	English
208	Navleen	Kaur	7	2021-09-01 06:30	Mathematics
208	Navleen	Kaur	7	2021-09-01 06:30	Mathematics
```
Explanation:  
UNION ALL combines the Student table with itself and retains duplicates. ORDER BY sorts by STUDENT_ID.

---

### 27. List STUDENT_ID who does not get Scholarship  
Query:
```sql
SELECT STUDENT_ID FROM Student 
WHERE STUDENT_ID NOT IN (SELECT STUDENT_REF_ID FROM Scholarship);
```
Output:
```
STUDENT_ID
204
205
206
207
208
```
Explanation:  
NOT IN identifies students whose IDs are not present in the Scholarship table.

---

### 28. Fetch the first 50% records from a table  
Query:
```sql
SELECT * 
FROM Student 
LIMIT (SELECT COUNT(*) FROM Student) / 2;
```

---

### 29. Fetch the MAJOR subjects that have less than 4 people  
Query:
```sql
SELECT MAJOR, COUNT(MAJOR) AS MAJOR_COUNT FROM Student GROUP BY MAJOR HAVING COUNT(MAJOR) < 4;
```
Output:
```
MAJOR	MAJOR_COUNT
Biology	1
Chemistry	1
Computer Science	1
English	1
History	1
Mathematics	2
Physics	1
```
Explanation:  
GROUP BY MAJOR and HAVING COUNT(MAJOR) < 4 filters majors with fewer than 4 students.

---

### 30. Show all MAJOR subjects along with the number of people in each  
Query:
```sql
SELECT MAJOR, COUNT(MAJOR) AS ALL_MAJOR FROM Student GROUP BY MAJOR;
```
Output:
```
MAJOR	ALL_MAJOR
Biology	1
Chemistry	1
Computer Science	1
English	1
History	1
Mathematics	2
Physics	1
```
Explanation:  
GROUP BY MAJOR counts students in each major.

---

### 31. Show the last record from a table  
Query:
```sql
SELECT * FROM Student WHERE STUDENT_ID = (SELECT MAX(STUDENT_ID) FROM STUDENT);
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
208	Navleen	Kaur	7	2021-09-01 06:30:00	Mathematics
```
Explanation:  
Select the row with the maximum STUDENT_ID to get the last record (assuming incremental IDs).

---

32. Fetch the first row of a table  
Query:
```sql
SELECT * FROM Student WHERE STUDENT_ID = (SELECT MIN(STUDENT_ID) FROM Student);
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
201	Shivansh	Mahajan	8.79	2021-09-01 09:30:00	Computer Science
```
Explanation:  
Select the row with the minimum STUDENT_ID to get the first record (assuming incremental IDs).

---

### 33. Fetch the last five records from a table  
Query:
```sql
SELECT * 
FROM (
    SELECT * 
    FROM Student 
    ORDER BY STUDENT_ID DESC 
    LIMIT 5
) AS subquery
ORDER BY STUDENT_ID;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
205	Kush	Kumar	7.85	2021-09-01 08:30:00	Physics
206	Prem	Chopra	9.56	2021-09-01 09:24:00	History
207	Pankaj	Vats	9.78	2021-09-01 02:30:00	English
208	Navleen	Kaur	7	2021-09-01 06:30:00	Mathematics
```
Explanation:  
Subquery retrieves last 5 by descending STUDENT_ID; outer query reorders in ascending STUDENT_ID.

---

### 34. Fetch three max GPA using correlated subquery  
Query:
```sql
SELECT DISTINCT GPA FROM Student S1 
WHERE 3 >= (SELECT COUNT(DISTINCT GPA) FROM Student S2 WHERE S1.GPA <= S2.GPA) 
ORDER BY S1.GPA DESC;
```
Output:
```
GPA
9.78
9.56
9.2
```
Explanation:  
The correlated subquery counts distinct GPAs greater than or equal to each GPA; when count ≤ 3, the GPA is among the top 3.

---

### 35. Fetch three min GPA using correlated subquery  
Query:
```sql
SELECT DISTINCT GPA FROM Student S1 
WHERE 3 >= (SELECT COUNT(DISTINCT GPA) FROM Student S2 WHERE S1.GPA >= S2.GPA) 
ORDER BY S1.GPA;
```
Output:
```
GPA
5.6
7
7.85
```
Explanation:  
Similar to the previous query but reversed to get the bottom 3 GPAs.

---

### 36. Fetch nth max GPA from a table (generic)  
Query:
```sql
SELECT DISTINCT GPA FROM Student S1 
WHERE n = (SELECT COUNT(DISTINCT GPA) FROM Student S2 WHERE S1.GPA <= S2.GPA) 
ORDER BY S1.GPA DESC;
```
Explanation:  
Replace n with the desired rank (e.g., 2 for second max). Uses correlated subquery counting distinct GPAs.

---

### 37. Fetch MAJOR subjects along with the max GPA in each MAJOR  
Query:
```sql
SELECT MAJOR, MAX(GPA) as MAXGPA FROM Student GROUP BY MAJOR;
```
Output:
```
MAJOR	MAXGPA
Biology	5.6
Chemistry	9.2
Computer Science	8.79
English	9.78
History	9.56
Mathematics	8.44
Physics	7.85
```
Explanation:  
GROUP BY MAJOR and MAX(GPA) gives the highest GPA per major.

---

### 38. Fetch the names of Students who have the highest GPA  
Query:
```sql
SELECT FIRST_NAME, GPA FROM Student WHERE GPA = (SELECT MAX(GPA) FROM Student);
```
Output:
```
FIRST_NAME	GPA
Pankaj	9.78
```
Explanation:  
Compare student GPA with the maximum GPA using a subquery.

---

### 39. Show the current date and time  
Query:
```sql
SELECT CURDATE(); -- To get the current date
SELECT NOW(); -- To get the current date and time
```
Output:
```
CURDATE()	NOW()
2025-01-16	2025-01-16 14:30:00
```
Explanation:  
CURDATE() returns date; NOW() returns date and time.

---

### 40. Create a new table copied from Student (structure + data)  
Query:
```sql
CREATE TABLE CloneTable AS SELECT * FROM Student;
```
Explanation:  
Creates a new table CloneTable with the same structure and contents as Student.

---

### 41. Update the GPA of all students in 'Computer Science' to 7.5  
Query:
```sql
UPDATE Student SET GPA = 7.5 WHERE MAJOR = 'Computer Science';
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
201	Shivansh	Mahajan	7.5	2021-09-01 09:30:00	Computer Science
```
Explanation:  
UPDATE with WHERE modifies GPA for the specified major.

---

### 42. Find the average GPA for each major  
Query:
```sql
SELECT MAJOR, AVG(GPA) AS AVERAGE_GPA FROM Student GROUP BY MAJOR;
```
Output:
```
MAJOR	AVERAGE_GPA
Biology	5.6
Chemistry	9.2
Computer Science	8.79
```
Explanation:  
AVG(GPA) grouped by MAJOR computes the average GPA per major.

---

### 43. Show the top 3 students with the highest GPA  
Query:
```sql
SELECT * FROM Student ORDER BY GPA DESC LIMIT 3;
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
207	Pankaj	Vats	9.78	2021-09-01 02:30:00	English
206	Prem	Chopra	9.56	2021-09-01 09:24:00	History
204	Radha	Sharma	9.2	2021-09-01 12:45:00	Chemistry
```
Explanation:  
ORDER BY GPA DESC with LIMIT 3 returns the top 3 students by GPA.

---

### 44. Find the number of students in each major who have a GPA greater than 7.5  
Query:
```sql
SELECT MAJOR, COUNT(STUDENT_ID) AS HIGH_GPA_COUNT FROM Student WHERE GPA > 3.5 GROUP BY MAJOR;
```
Output:
```
MAJOR	HIGH_GPA_COUNT
Biology	1
Chemistry	1
Computer Science	1
English	1
History	1
Mathematics	2
Physics	1
```
Explanation:  
Filter by GPA threshold and GROUP BY MAJOR to count students meeting the criterion. (Note: the WHERE clause shown uses 3.5 in the source; for >7.5 use WHERE GPA > 7.5.)

---

### 45. Find students who have the same GPA as 'Shivansh Mahajan'  
Query:
```sql
SELECT * FROM Student WHERE GPA = (SELECT GPA FROM Student WHERE FIRST_NAME = 'Shivansh' 
AND LAST_NAME = 'Mahajan');
```
Output:
```
STUDENT_ID	FIRST_NAME	LAST_NAME	GPA	ENROLLMENT_DATE	MAJOR
201	Shivansh	Mahajan	4	2021-09-01 09:30:00	Computer Science
```
Explanation:  
A subquery fetches the GPA of 'Shivansh Mahajan' and the outer query retrieves all students with that GPA.

---

`Notes:`
- Some queries and outputs above are written for illustrative purposes and may depend on SQL dialect (MySQL, PostgreSQL, Oracle, etc.). Functions like SUBSTRING, INSTR, LENGTH, CURDATE, NOW may vary by RDBMS.
- Replace "n" in generic queries with the numeric rank you need.
- For production use, ensure proper indexing when performing joins and aggregations on large tables.

If you want this README converted to a different format (HTML, PDF) or adjusted for a specific SQL dialect, tell me which dialect and I will adapt the queries and functions accordingly.
