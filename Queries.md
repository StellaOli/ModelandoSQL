## Inserção de vinculo medico-paciente: 

```sql

    INSERT INTO paciente_medico (paciente_id, medico_id)
    VALUES (
        1021656097075888129, 
        (SELECT medico_id FROM Medico ORDER BY medico_id ASC LIMIT 1)
    );
    
    
    INSERT INTO paciente_medico (paciente_id, medico_id)
    VALUES (
        1021656097075888129, 
        (SELECT medico_id FROM Medico ORDER BY medico_id ASC OFFSET 1 LIMIT 1)
    );
    
    
    INSERT INTO paciente_medico (paciente_id, medico_id)
    VALUES (
        1021656097075888129, 
        (SELECT medico_id FROM Medico ORDER BY medico_id ASC OFFSET 2 LIMIT 1)
    );

    
    INSERT INTO paciente_medico (paciente_id, medico_id)
    VALUES (
        (SELECT paciente_id FROM Paciente ORDER BY paciente_id ASC LIMIT 1),
        1021656099481419777
    );
    
    
    INSERT INTO paciente_medico (paciente_id, medico_id)
    VALUES (
        (SELECT paciente_id FROM Paciente ORDER BY paciente_id ASC OFFSET 1 LIMIT 1),
        1021656099481419777
    );
    
    
    INSERT INTO paciente_medico (paciente_id, medico_id)
    VALUES (
        (SELECT paciente_id FROM Paciente ORDER BY paciente_id ASC OFFSET 2 LIMIT 1),
        1021656099481419777
    );
```

### 1. Quais médicos já atenderam o paciente "Raul Montenegro"?

```sql
SELECT nome_m
FROM medico m
 
JOIN consulta_medico cm ON m.medico_id = cm.medico_id
JOIN consulta c ON cm.consulta_id = c.consulta_id
JOIN status_consulta sc ON c.status_id = sc.status_id
JOIN paciente_consulta pc ON c.consulta_id = pc.consulta_id
JOIN paciente p ON pc.paciente_id = p.paciente_id

WHERE p.nome_p ILIKE 'Raul Montenegro' AND sc.descricao_status ILIKE 'Realizada'
````

### 2. Quantas consultas existem em cada status (agendada, realizada, cancelada)?
```sql
SELECT sc.descricao_status, COUNT(c.status_id) AS total_consultas
FROM consulta c

JOIN status_consulta sc ON c.status_id = sc.status_id
GROUP BY sc.descricao_status
```

### 3. Qual especialidade médica tem mais consultas realizadas?
```sql
SELECT especialidade,
COUNT(*) AS total
FROM medico m 

JOIN consulta_medico cm ON m.medico_id = cm.medico_id
JOIN consulta c ON cm.consulta_id = c.consulta_id
JOIN status_consulta sc ON c.status_id = sc.status_id

WHERE sc.descricao_status ILIKE 'Realizada'
GROUP BY especialidade
LIMIT 1
```

### 4. Quais são os 5 remédios mais prescritos?
```sql
SELECT nome_remedio, 
COUNT(*) AS total 
FROM receita_remedio rr
 
GROUP BY nome_remedio
ORDER BY total DESC
LIMIT 5;
```

### 5. Quantas consultas os pacientes já realizaram?
```sql
SELECT p.nome_p, COUNT(pc.paciente_id) AS total
FROM paciente p
 
JOIN paciente_consulta pc ON p.paciente_id = pc.paciente_id
JOIN consulta c ON pc.consulta_id = c.consulta_id
JOIN status_consulta sc ON c.status_id = sc.status_id

WHERE sc.descricao_status ILIKE 'Realizada'
GROUP BY nome_p
ORDER BY total desc
```


### 6. Quais consultas o médico "Gustavo da Mota" realizou nos ultimos 6 meses?
```sql
SELECT c.consulta_id, c.data, c.status_id
FROM consulta c

JOIN consulta_medico cm ON c.consulta_id = cm.consulta_id
JOIN medico m ON cm.medico_id = m.medico_id
JOIN status_consulta sc ON c.status_id = sc.status_id

WHERE m.nome_m = 'Gustavo da Mota' 
AND sc.descricao_status = 'Realizada' 
AND c.data >= NOW() - INTERVAL '6 month'
ORDER BY c.data DESC;
```

### 7. Quais remedios a paciente "Isadora Marques" já foi receitada?
```sql
SELECT r.nome_remedio
FROM remedio r

JOIN receita_remedio rr ON r.nome_remedio = rr.nome_remedio
JOIN receita re ON rr.receita_id = re.receita_id
JOIN receita_consulta rc ON re.receita_id = rc.receita_id
JOIN consulta c ON rc.consulta_id = c.consulta_id
JOIN paciente_consulta pc ON c.consulta_id = pc.consulta_id
JOIN paciente p ON pc.paciente_id = p.paciente_id
WHERE p.nome_p ILIKE 'Isadora Marques'
```

### 8. Quais médicos atendem pacientes do convênio "Unimed"?
```sql
SELECT DISTINCT m.medico_id, m.nome_m
FROM medico m

JOIN consulta_medico cm ON m.medico_id = cm.medico_id
JOIN consulta c ON cm.consulta_id = c.consulta_id
JOIN paciente_consulta pc ON c.consulta_id = pc.consulta_id
JOIN paciente p ON pc.paciente_id = p.paciente_id
WHERE p.convenio ILIKE 'Unimed'
```


### 9. Qual convenio tem a maior quantidade de consultas canceladas?
```sql
SELECT convenio, COUNT(c.status_id) AS total
FROM paciente p
 
JOIN paciente_consulta pc ON p.paciente_id = pc.paciente_id
JOIN consulta c ON pc.consulta_id = c.consulta_id
JOIN status_consulta sc ON c.status_id = sc.status_id
WHERE sc.descricao_status ILIKE 'Cancelada'
GROUP BY convenio
```


### 10. Quais remédios cadastrados são de cada tipo de tarja?
```sql
SELECT nome_remedio, tarja
FROM remedio r 

GROUP BY tarja, nome_remedio
ORDER BY nome_remedio asc
```
