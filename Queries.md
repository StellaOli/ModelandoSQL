1. Quais médicos já atenderam o paciente "João Silva"?
select nome_m
from medico m 
join consulta_medico cm on m.medico_id = cm.medico_id
join consulta c on cm.consulta_id = c.consulta_id
join status_consulta sc on c.status_id = sc.status_id
join paciente_consulta pc on c.consulta_id = pc.consulta_id
join paciente p on pc.paciente_id = p.paciente_id
where p.nome_p ilike 'João Silva' and sc.descricao_status ilike 'Realizada'

2. Quantas consultas existem em cada status (agendada, realizada, cancelada)?
SELECT descricao_status, 
COUNT(*) AS total
FROM status_consulta sc 
GROUP BY descricao_status;

3. Qual especialidade médica tem mais consultas realizadas?
select 
especialidade,
count(*) as total

from 
medico m 

join consulta_medico cm on m.medico_id = cm.medico_id
join consulta c on cm.consulta_id = c.consulta_id
join status_consulta sc on c.status_id = sc.status_id
where sc.descricao_status ilike 'Realizada'

4. Quais são os 5 remédios mais prescritos?

SELECT nome_remedio, 
COUNT(*) AS total 
FROM receita_remedio rr 
GROUP BY nome_remedio
ORDER BY total DESC
LIMIT 5;


5. Quais pacientes realizaram o maior número de consultas?
select nome_p
from paciente p 
join paciente_consulta pc on p.consulta_id = pc.consulta_id
join consulta c on pc.consulta_id = c.consulta_id
join status_consulta sc on c.consulta_id = sc.consulta_id

select paciente_id
count(*) as total
from paciente_consulta pc 

where sc.descricao_status ilike 'Realizada'
order by total desc



6. Quais consultas o médico "Dra. Ana Souza" realizou no último mês?
SELECT 
    c.consulta_id, 
    c.data , 
    c.status_id
FROM consultas c
JOIN medicos m ON c.medico_id = m.medico_id
join status_consulta sc on c.status_id = sc.status_id
WHERE 
    m.nome_medico = 'Dra. Ana Souza'
    AND c.descricao_status = 'realizada'
    AND c.data >= DATE_SUB(CURDATE(), INTERVAL 1 MONTH)
ORDER BY 
    c.data DESC;


7. Quais remedios a paciente "Maria Oliveira" já foi receitada?
select nome_remedio
from remedio r 
join receita_remedio rr on r.nome_remedio = rr.nome_remedio
join receita r on rr.receita_id = r.receita_id
join receita_consulta rc on r.receita_id = rc.receita_id
join consulta c on rc.consulta_id = c.consulta_id
join paciente_consulta pc on c.consulta_id = pc.consulta_id
join paciente p on pc.paciente_id = p.paciente_id
where p.nome_p ilike 'Maria Oliveira'


8. Quais médicos atendem pacientes do convênio "Unimed"?
SELECT DISTINCT 
    m.id_medico, 
    m.nome_medico
FROM medico m
JOIN consulta_medico cm ON m.medico_id = cm.medico_id
join consulta c on cm.consulta_id = c.consulta_id
JOIN pacientes p ON c.paciente_id = p.paciente_id
WHERE p.convenio ilike 'Unimed'



9. Qual o número total de consultas e receitas geradas?
SELECT 
(SELECT COUNT(*) FROM consulta) AS total_consultas,
(SELECT COUNT(*) FROM receita) AS total_receitas;



10. Quais remédios cadastrados são de cada tipo de tarja?
select nome_remedio,tarja

from remedio r 

group by tarja, nome_remedio
order by nome_remedio asc
