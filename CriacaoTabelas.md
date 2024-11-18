```sql
-- Tabela Paciente
CREATE TABLE Paciente (
    paciente_id SERIAL PRIMARY KEY,
    nome_p VARCHAR(50),
    data_nasc TIMESTAMP,
    convenio VARCHAR(50),
    cpf VARCHAR(11) UNIQUE,
    telefone VARCHAR(15)
);

-- Tabela Medico
CREATE TABLE Medico (
    medico_id SERIAL PRIMARY KEY,
    crm VARCHAR(6),
    nome_m VARCHAR(50),
    especialidade VARCHAR(50)
);

-- Tabela Receita
CREATE TABLE Receita (
    receita_id SERIAL PRIMARY KEY,
    data TIMESTAMP,
    tipo_receita VARCHAR(30)
);

-- Tabela Remedio
CREATE TABLE Remedio (
    nome_remedio VARCHAR(50) PRIMARY KEY,
    tarja VARCHAR(30),
    dosagem FLOAT
);

-- Tabela Status_Consulta
CREATE TABLE Status_Consulta (
    status_id SERIAL PRIMARY KEY,
    descricao_status VARCHAR(50) NOT NULL UNIQUE
);

-- Tabela Consulta (com consulta_id)
CREATE TABLE Consulta (
    consulta_id SERIAL PRIMARY KEY,
    data TIMESTAMP,
    horario VARCHAR(30),
    status_id INT REFERENCES Status_Consulta(status_id) DEFAULT 1
);

-- Tabela Admin
CREATE TABLE Admin (
    adm_id SERIAL PRIMARY KEY,
    "username" VARCHAR(30),
    senha VARCHAR(128),
    email VARCHAR(50)
);

-- Tabela Paciente_Consulta (com consulta_id)
CREATE TABLE Paciente_Consulta (
    paciente_id INT REFERENCES Paciente(paciente_id),
    consulta_id INT REFERENCES Consulta(consulta_id),
    PRIMARY KEY (paciente_id, consulta_id)
);

-- Tabela Paciente_Medico
CREATE TABLE Paciente_Medico (
    paciente_id INT REFERENCES Paciente(paciente_id),
    medico_id INT REFERENCES Medico(medico_id),
    PRIMARY KEY (paciente_id, medico_id)
);

-- Tabela Consulta_Medico (com consulta_id)
CREATE TABLE Consulta_Medico (
    consulta_id INT REFERENCES Consulta(consulta_id),
    medico_id INT REFERENCES Medico(medico_id),
    PRIMARY KEY (consulta_id, medico_id)
);

-- Tabela Receita_Consulta (com consulta_id)
CREATE TABLE Receita_Consulta (
    receita_id INT REFERENCES Receita(receita_id),
    consulta_id INT REFERENCES Consulta(consulta_id),
    PRIMARY KEY (receita_id, consulta_id)
);

-- Tabela Receita_Remedio
CREATE TABLE Receita_Remedio (
    receita_id INT REFERENCES Receita(receita_id),
    nome_remedio VARCHAR(50) REFERENCES Remedio(nome_remedio),
    quantidade INT,
    PRIMARY KEY (receita_id, nome_remedio)
);

-- Inserindo Status Padrão na Tabela Status_Consulta
INSERT INTO Status_Consulta (descricao_status) VALUES 
('Agendada'), 
('Realizada'), 
('Cancelada'), 
('Reagendada');

