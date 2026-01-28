A)       
  
SELECT          E.name,B.bonus   
FROM            Employee AS E                 
LEFT JOIN       Bonus B             
ON              E.empId=B.empId                 
WHERE           B.bonus<1000 OR B.empId is NULL        

---   

