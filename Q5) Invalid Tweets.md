A)
SELECT tweet_id   
FROM Tweets   
WHERE CHAR_LENGTH(content) > 15   
ORDER BY tweet_id;   


.len / .length doesn’t work   
Those are pandas / Python concepts.  
 
In SQL, string length is done using functions:  
MySQL: CHAR_LENGTH(str) (or LENGTH(str) but that counts bytes)   
SQL Server: LEN(str)  
PostgreSQL: LENGTH(str)   
Leetcode uses MySQL   
    
