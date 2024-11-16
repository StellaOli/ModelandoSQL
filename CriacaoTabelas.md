```sql
CREATE TABLE Paciente (
    paciente_id SERIAL PRIMARY KEY,
    nome_p VARCHAR(50),
    data_nasc TIMESTAMP,
    convenio VARCHAR(50)
);

CREATE TABLE Medico (
    medico_id SERIAL PRIMARY KEY,
    crm VARCHAR(6),
    nome_m VARCHAR(50),
    especialidade VARCHAR(50)
);

CREATE TABLE Pedido_Exame (
    num_pedido SERIAL PRIMARY KEY,
    data TIMESTAMP,
    horario VARCHAR(30),
    tipo_exame VARCHAR(30)
);

CREATE TABLE Exame (
    nome_exame VARCHAR(150) PRIMARY KEY,
    categoria INT,
    codigo_exame INT
);

CREATE TABLE Agend_Exame (
    exame_id SERIAL PRIMARY KEY,
    data TIMESTAMP,
    horario VARCHAR(30)
);

CREATE TABLE Receita (
    receita_id SERIAL PRIMARY KEY,
    data TIMESTAMP,
    tipo_receita VARCHAR(30)
);

CREATE TABLE Remedio (
    nome_remedio VARCHAR(50) PRIMARY KEY,
    tarja VARCHAR(30),
    dosagem FLOAT
);

CREATE TABLE Consulta (
    codigo_con SERIAL PRIMARY KEY,
    data TIMESTAMP,
    horario VARCHAR(30)
);

CREATE TABLE Admin (
    adm_id SERIAL PRIMARY KEY,
    "username" VARCHAR(30),
    senha VARCHAR(30)
);
CREATE TABLE Paciente_Consulta (
    paciente_id INT REFERENCES Paciente(paciente_id),
    consulta_id INT REFERENCES Consulta(codigo_con),
    PRIMARY KEY (paciente_id, consulta_id)
);
