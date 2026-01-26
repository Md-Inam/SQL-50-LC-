We need to compare a row with another row in the same table. That means a self join.      
A self-join can create more rows than are in the original table   

When you write:     
FROM Weather W1         
JOIN Weather W2           
           
SQL treats this as:          
Take every row from W1                           
Pair it with every row from W2             
Then keep only the pairs that satisfy the ON condition             

---  
        ANSWER:
SELECT         W1.id      
FROM           Weather AS W1          
JOIN           Weather AS W2                     
ON             W1.recordDate=W2.recordDate+INTERVAL 1 DAY            
WHERE          W1.temperature>W2.temperature         

        (OR)  
SELECT W1.id   
FROM Weather AS W1        
JOIN Weather AS W2                             
ON W1.recordDate = DATE_ADD(W2.recordDate, INTERVAL 1 DAY)           
WHERE W1.temperature > W2.temperature;       

---  


If the table has N rows, the raw join space is:     
N × N rows          
That’s called a Cartesian product.          

Why this doesn’t usually explode in real queries  
Because we almost always add a condition: 
ON w1.recordDate = w2.recordDate + INTERVAL 1 DAY 
That condition filters the N×N combinations down to only meaningful pairs  

