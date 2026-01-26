A)  
SELECT V.customer_id , Count(*) AS count_no_trans              
FROM Visits AS V              
LEFT JOIN Transactions AS T             
ON V.visit_id=T.visit_id         
WHERE T.visit_id IS NULL           
GROUP BY V.customer_id          
   



If each row represents an event (a visit, order, login, etc.)  
then COUNT(*) = number of those events  

When we write:  
GROUP BY customer_id  
SQL groups rows like this internally:   
Customer 54 → [visit 7, visit 8]  

---

| Clause   | Job                        |
| -------- | -------------------------- |
| GROUP BY | Make buckets               |
| COUNT    | Count items in each bucket |
| ORDER BY | Arrange final rows         |

---  


Problem:  
We want:  
Customers who had visits where no transaction happened,  
and how many such visits they had.   

---
  
Whenever the question asks for:  
did NOT buy           
has NO orders        
missing records         
never did X             
Means:                    
“I need rows that exist in one table but NOT in another.”         
That is called an ANTI-JOIN problem 


---  


INNER JOIN only keeps visits that DO have transactions. 
We want visits without transactions.    

