Just the division part  
SUM(action = 'confirmed') / COUNT(*)  


Add rounding   
ROUND(SUM(action = 'confirmed') / COUNT(*), 2)   

You want:

(confirmed ÷ total), rounded to 2 decimals, and if result is NULL → make it 0  

ROUND( IFNULL( value , 0 ) , 2 )  

---

SELECT Signups.user_id, ROUND(IFNULL(SUM(Confirmations.action='confirmed')/COUNT(Confirmations.action),0),2) AS confirmation_rate
FROM   Signups
LEFT JOIN Confirmations
ON Signups.user_id=Confirmations.user_id
GROUP BY Signups.user_id  

--- 

| With GROUP BY       | Without GROUP BY            |
| ------------------- | --------------------------- |
| One result per user | One result for entire table |
| user_id makes sense | user_id is ambiguous        |

  
