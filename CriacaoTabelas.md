```sql
CREATE TABLE "Paciente" (
    paciente_id SERIAL PRIMARY KEY,
    nome_p VARCHAR(50),
    data_nasc TIMESTAMP,
    convenio VARCHAR(50)
);

CREATE TABLE "Medico" (
    medico_id INT PRIMARY KEY,
    crm VARCHAR(6),
    nome_m VARCHAR(50),
    especialidade VARCHAR(50)
);

CREATE TABLE "Pedido_Exame" (
    num_pedido INT PRIMARY KEY,
    data TIMESTAMP,
    horario VARCHAR(4),
    tipo_exame VARCHAR(30)
);

CREATE TABLE "Exame" (
    nome_exame VARCHAR(50) PRIMARY KEY,
    categoria INT,
    codigo_exame INT
);

CREATE TABLE "Agend_Exame" (
    exame_id INT PRIMARY KEY,
    data TIMESTAMP,
    horario VARCHAR(4)
);

CREATE TABLE "Receita" (
    receita_id INT PRIMARY KEY,
    data TIMESTAMP,
    tipo_receita VARCHAR(7)
);

CREATE TABLE "Remedio" (
    nome_remedio VARCHAR(50) PRIMARY KEY,
    tarja VARCHAR(7),
    dosagem FLOAT
);

CREATE TABLE "Consulta" (
    codigo_con INT PRIMARY KEY,
    data TIMESTAMP,
    horario VARCHAR(4)
);

CREATE TABLE "Admin" (
    adm_id INT PRIMARY KEY,
    "user" VARCHAR(15),
    senha VARCHAR(8)
);

