A)
   
SELECT       A1.machine_id,         
                           ROUND(AVG(A2.timestamp-A1.timestamp),3) AS processing_time                
FROM         Activity AS A1           
JOIN         Activity AS A2              
ON           A1.machine_id=A2.machine_id AND A1.process_id=A2.process_id            
WHERE        A1.activity_type="start" and A2.activity_type="end"            
GROUP BY     A1.machine_id            
        

---

GROUP BY = “What level do you want ONE ROW per?”
Ask:
“What does ONE row in my final answer represent?


---
  
1️⃣ Each process has two rows: a start event and an end event. 
 
2️⃣ We self join the table to pair rows from the same machine and process.  
 
3️⃣ We filter to keep only start → end pairs.  

4️⃣ Duration per process = end timestamp − start timestamp.  
  
5️⃣ We GROUP BY machine_id and take the AVG(duration) to get each machine’s processing time.  
