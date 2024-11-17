## 4. Quais são os 5 remédios mais prescritos?

    SELECT 
      nome_remedio, 
      COUNT(*) AS total 
    FROM 
      receita_remedio rr 
    GROUP BY 
      nome_remedio
    ORDER BY 
      total DESC
    LIMIT 5;
