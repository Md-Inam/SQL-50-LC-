A)
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id;
-------------------------------
To permanently change the name of a column in SQL, you must use the ALTER TABLE statement       
-> ALTER TABLE table_name             
-> RENAME COLUMN old_column_name TO new_column_name;       

---  
(OR) 

---  
  
-> ALTER TABLE Customers RENAME COLUMN fname TO first_name;              
But Alter is DDL , cant be used with DQL
FIX:
-> AS creates an alias: a temporary name for a column or table in the result/output of the query.     -> It does not rename the real database column


 --- 
 
-> schema changes are:
risky
permanent
not allowed in most environments
forbidden in coding platforms
forbidden in production without change review

--- 
  
-> Whereas aliasing is:
safe
temporary
standard
readable
professional

