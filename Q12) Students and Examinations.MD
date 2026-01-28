 A)  

SELECT s.student_id, s.student_name, sub.subject_name,  COUNT(e.student_id) AS attended_exams            
FROM      Students s          
CROSS     JOIN Subjects sub         
LEFT JOIN Examinations e                
ON        e.student_id = s.student_id            
AND       e.subject_name = sub.subject_name             
GROUP BY  s.student_id, s.student_name, sub.subject_name           
ORDER BY  s.student_id, sub.subject_name;         


 
(OR) 

SELECT S.student_id, S.student_name, Sb.Subject_name, COUNT(E.student_id) AS attended_exams 
FROM (Students S, Subjects Sb ) 
LEFT JOIN Examinations E   
ON S.student_id = E.student_id AND Sb.Subject_name = E.Subject_name 
GROUP BY S.student_id, S.student_name, Sb.Subject_name  
ORDER BY S.student_id, Sb.Subject_name  
 
--- 

“number of times each student attended each exam”    

“Each exam” does NOT mean “only exams that happened.”  
It means each exam subject that exists in the system.   

For every student, for every subject offered, count how many exam records exist.  

---   

INNER = matches only
LEFT = keep left side
RIGHT = keep right side
FULL = keep both sides
CROSS = all combinations
  
CROSS JOIN does not use ON  


---  

If you run:   
SELECT COUNT(*) FROM table;  

Example without GROUP BY

Table:
student	subject
Alice	  Math
Alice	  Math
Alice	  Physics
Bob	    Math
    
COUNT   
4    
All rows treated as one big group.   


Example WITH:
GROUP BY student

SELECT student, COUNT(*) 
FROM table
GROUP BY student;


SQL makes buckets:

Alice bucket → 3 rows  
Bob bucket → 1 row  
  
Result:  
  
| student | count |  
| ------- | ----- | 
| Alice   | 3     |  
| Bob     | 1     |  


