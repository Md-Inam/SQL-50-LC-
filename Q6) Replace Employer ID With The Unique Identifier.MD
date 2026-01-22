A)        
SELECT      U.unique_id,E.name               
FROM        Employees as E             
LEFT JOIN   EmployeeUNI as U             
ON          E.id=U.id         

-----  

When you use a **JOIN** in SQL
It creates a temporary combined result that contains columns from **both tables** for the matched rows.  
The type of join (like **INNER** or **LEFT**) decides **which rows stay** in that combined result.    
After joining, you can access any column using table aliases (like `e.name`, `u.unique_id`).   
Finally, the **SELECT** statement decides which columns are actually shown in the output. 
