5 reports = 5 employees whose managerId equals that manager’s id  

You cannot use COUNT() in WHERE.   
WHERE filters rows.  
COUNT() works after grouping.

---  

SQL order of operations:

WHERE filters rows  
GROUP BY makes groups ← you skipped this   
COUNT happens per group   
HAVING filters groups  


WHERE filters rows before grouping  
HAVING filters groups after counting  
 ---

A.1) 
A subquery is just a query inside another query. Usually inside ().   

Managers who have atleast 5 direct reports

SELECT managerId    
FROM Employee   
WHERE managerId IS NOT NULL  
GROUP BY managerId   
HAVING COUNT(managerId) >=5;


---
A1) 
Other Half(Program Watns Names)    

SELECT name  
FROM Employee   
WHERE id IN (  
    SELECT managerId  
    FROM Employee   
    WHERE managerId IS NOT NULL  
    GROUP BY managerId  
    HAVING COUNT(*) >= 5  
) 



Database relationships run on keys  
You do want the ID in the subquery because IDs link rows. Names are just labels. Labels can repeat. IDs are unique  

We use id IN (subquery) because:

managerId refers to id  
That is the relationship  






