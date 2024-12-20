# Código: COMO USAR ▶️

#### Resgate sua conection string do banco de dados CockRoach e substitua nos campos indicados. 
#### Altere os numeros da variável *id_status* para os id devolvidos repectivamente na Query --> 

```sql
    
    select status_id from status_consulta where descricao_status = 'Agendada';
    select status_id from status_consulta where descricao_status = 'Realizada';
    select status_id from status_consulta where descricao_status = 'Cancelada';
    select status_id from status_consulta where descricao_status = 'Reagendada';
    
```

#### O campo de *id_status* está indicado no código com: ┏─══─━══─⊱✠⊰─══━─══─┓ e Setas ->

#### Após isso, apenas rode o código, e ele devolverá mensagens durante as inserções;

----

```python
from faker import Faker
import random
import psycopg2  
fake = Faker('pt_BR')

convenios_reais = ["Amil", "Bradesco Saúde", "Unimed", "SulAmérica Saúde", "NotreDame Intermédica"]
remedios_reais = [
    {"nome": "Paracetamol", "tarja": "Sem Tarja", "dosagem": 500},
    {"nome": "Ibuprofeno", "tarja": "Sem Tarja", "dosagem": 400},
    {"nome": "Losartana", "tarja": "Tarja Vermelha", "dosagem": 50},
    {"nome": "Fluoxetina", "tarja": "Tarja Vermelha", "dosagem": 20},
    {"nome": "Omeprazol", "tarja": "Sem Tarja", "dosagem": 20},
]

especialidades_reais = [
    "Cardiologia", "Pediatria", "Ortopedia", "Dermatologia", "Neurologia", 
    "Ginecologia", "Oftalmologia", "Endocrinologia", "Psiquiatria", "Urologia"
]

# Configuração do banco de dados
db_config = {
    "dbname": " ",  # Substitua pelo nome do seu banco
    "user": " ",  # Substitua pelo seu usuário
    "password": " ",  # Substitua pela sua senha
    "host": " ",  # Substitua pelo host do seu banco
    "port": 26257,
}

def truncate(value, max_length):
    return value[:max_length] if len(value) > max_length else value

def populate_database():
    try:
        conn = psycopg2.connect(**db_config)
        cursor = conn.cursor()

        paciente_ids = []
        for _ in range(10):  
            nome_p = truncate(fake.name(), 50)
            data_nasc = fake.date_of_birth(minimum_age=18, maximum_age=90)
            convenio = truncate(random.choice(convenios_reais), 50)
            cpf = fake.cpf().replace('.', '').replace('-', '')
            telefone = truncate(fake.phone_number(), 15)
            cursor.execute(
                "INSERT INTO Paciente (nome_p, data_nasc, convenio, cpf, telefone) VALUES (%s, %s, %s, %s, %s) RETURNING paciente_id",
                (nome_p, data_nasc, convenio, cpf, telefone),
            )
            paciente_id = cursor.fetchone()[0]  
            paciente_ids.append(paciente_id)
            print(f"Paciente {nome_p} inserido com sucesso, ID: {paciente_id}")

        medico_ids = [] 
        for _ in range(5):  
            nome_m = truncate(fake.name(), 50)
            crm = str(fake.random_int(min=100000, max=999999))
            especialidade = truncate(random.choice(especialidades_reais), 50)  
            cursor.execute(
                "INSERT INTO Medico (crm, nome_m, especialidade) VALUES (%s, %s, %s) RETURNING medico_id",
                (crm, nome_m, especialidade),
            )
            medico_id = cursor.fetchone()[0]  
            medico_ids.append(medico_id)  
            print(f"Médico {nome_m} inserido com sucesso, ID: {medico_id}")

        for remedio in remedios_reais:
            cursor.execute(
                "INSERT INTO Remedio (nome_remedio, tarja, dosagem) VALUES (%s, %s, %s) ON CONFLICT (nome_remedio) DO NOTHING",
                (truncate(remedio["nome"], 50), truncate(remedio["tarja"], 30), remedio["dosagem"]),
            )
            print(f"Remédio {remedio['nome']} inserido com sucesso.")

        #┏─══─━══─⊱✠⊰─══━─══──══─━══─⊱✠⊰─══━─══──══─━══─⊱✠⊰─══━─══──══─━══─⊱✠⊰─══━─══──══─━══─⊱✠⊰─══━─══──══─━══─⊱✠⊰─══━─══─┓
        # ALTERAR CONSULTA STATUS 
        consulta_ids = []
        for _ in range(20):  
            data = fake.date_this_year()
            horario = truncate(fake.time(), 30)
            id_status = [1021654625661616129, 1021654625661681665, 1021654625661714433, 1021654625661747201]  # ALTERAR PARA O ID CRIADO PELO BANCO NA TABELA, RESPECTIVAMENTE -->  SELECT * FROM status_consulta;
            status_id = random.choice(id_status)  
            cursor.execute(
                "INSERT INTO Consulta (data, horario, status_id) VALUES (%s, %s, %s) RETURNING consulta_id",
                (data, horario, status_id),
            )
            consulta_id = cursor.fetchone()[0] 
            consulta_ids.append(consulta_id)
            print(f"Consulta inserida com sucesso, ID: {consulta_id}")
            paciente_id = random.choice(paciente_ids)  
            cursor.execute(
                "INSERT INTO Paciente_Consulta (paciente_id, consulta_id) VALUES (%s, %s)",
                (paciente_id, consulta_id),
            )

            medico_id = random.choice(medico_ids) 
            cursor.execute(
                "INSERT INTO Consulta_Medico (consulta_id, medico_id) VALUES (%s, %s)",
                (consulta_id, medico_id),
            )

        for _ in range(15):  
            data = fake.date_this_year()
            tipo_receita = truncate(random.choice(["Controle Especial", "Simples"]), 30)
            cursor.execute(
                "INSERT INTO Receita (data, tipo_receita) VALUES (%s, %s) RETURNING receita_id",
                (data, tipo_receita),
            )
            receita_id = cursor.fetchone()[0]  
            print(f"Receita inserida com sucesso, ID: {receita_id}")

            consulta_id = random.choice(consulta_ids) 
            cursor.execute(
                "INSERT INTO Receita_Consulta (receita_id, consulta_id) VALUES (%s, %s)",
                (receita_id, consulta_id),
            )

            for _ in range(random.randint(1, 3)):  
                remedio = random.choice(remedios_reais)
                quantidade = random.randint(1, 10)
                cursor.execute(
                    """
                    INSERT INTO Receita_Remedio (receita_id, nome_remedio, quantidade) 
                    VALUES (%s, %s, %s)
                    ON CONFLICT (receita_id, nome_remedio) DO NOTHING
                    """,
                    (receita_id, truncate(remedio["nome"], 50), quantidade),
                )
            print(f"Remédios associados à receita {receita_id} com sucesso.")
        conn.commit()
        print("Todos os dados foram inseridos com sucesso.")

    except Exception as e:
        print(f"Erro ao inserir dados: {e}")
    finally:
        if conn:
            cursor.close()
            conn.close()

if __name__ == "__main__":
    populate_database()

```
