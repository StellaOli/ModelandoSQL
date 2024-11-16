# Código 

```python
import psycopg2
from faker import Faker
import random

# Conexão com o banco de dados CockroachDB
conn = psycopg2.connect(
    host="hard-beast-11738.6wr.aws-us-west-2.cockroachlabs.cloud",  # Substitua pelo host do seu banco
    port="26257",  # Porta padrão do CockroachDB
    dbname="ProjetoDB02",  # Substitua pelo nome do seu banco
    user="luiis",  # Substitua pelo seu usuário
    password="qqQ-TYVg8HhXKo3MYl8YXQ"  # Substitua pela sua senha
)
cur = conn.cursor()

fake = Faker()

# Função para inserir dados fictícios na tabela Paciente
def insert_paciente():
    nome_p = fake.name()
    data_nasc = fake.date_of_birth(minimum_age=18, maximum_age=80)
    convenio = fake.company()
    query = f"""
        INSERT INTO Paciente (nome_p, data_nasc, convenio)
        VALUES ('{nome_p}', '{data_nasc}', '{convenio}');
    """
    cur.execute(query)

# Função para inserir dados fictícios na tabela Medico
def insert_medico():
    crm = fake.unique.bothify(text='######')
    nome_m = fake.name()
    especialidade = random.choice(['Cardiologia', 'Dermatologia', 'Neurologia', 'Pediatria', 'Ortopedia'])
    query = f"""
        INSERT INTO Medico (crm, nome_m, especialidade)
        VALUES ('{crm}', '{nome_m}', '{especialidade}');
    """
    cur.execute(query)

# Função para inserir dados fictícios na tabela Pedido_Exame
def insert_pedido_exame():
    num_pedido = fake.unique.random_int(min=1000, max=9999)
    data = fake.date_this_decade()
    horario = fake.time()
    tipo_exame = random.choice(['Exame de sangue', 'Raio-X', 'Ultrassom', 'Tomografia'])
    query = f"""
        INSERT INTO Pedido_Exame (num_pedido, data, horario, tipo_exame)
        VALUES ({num_pedido}, '{data}', '{horario}', '{tipo_exame}');
    """
    cur.execute(query)

# Função para inserir dados fictícios na tabela Exame
def insert_exame():
    nome_exame = fake.unique.word()  # Garante que o nome_exame seja único
    categoria = random.choice([1, 2, 3, 4])
    codigo_exame = fake.unique.random_int(min=1000, max=9999)
    query = f"""
        INSERT INTO Exame (nome_exame, categoria, codigo_exame)
        VALUES ('{nome_exame}', {categoria}, {codigo_exame});
    """
    cur.execute(query)


# Função para inserir dados fictícios na tabela Agend_Exame
def insert_agend_exame():
    exame_id = fake.unique.random_int(min=1000, max=9999)
    data = fake.date_this_year()
    horario = fake.time()
    query = f"""
        INSERT INTO Agend_Exame (exame_id, data, horario)
        VALUES ({exame_id}, '{data}', '{horario}');
    """
    cur.execute(query)

# Função para inserir dados fictícios na tabela Receita
def insert_receita():
    receita_id = fake.unique.random_int(min=1000, max=9999)
    data = fake.date_this_year()
    tipo_receita = random.choice(['Antibiótico', 'Anti-inflamatório', 'Analgésico', 'Antidepressivo'])
    query = f"""
        INSERT INTO Receita (receita_id, data, tipo_receita)
        VALUES ({receita_id}, '{data}', '{tipo_receita}');
    """
    cur.execute(query)

# Função para inserir dados fictícios na tabela Remedio
def insert_remedio():
    nome_remedio = fake.unique.word()
    tarja = random.choice(['Preta', 'Vermelha', 'Amarela', 'Sem tarja'])
    dosagem = round(random.uniform(1.0, 500.0), 1)
    query = f"""
        INSERT INTO Remedio (nome_remedio, tarja, dosagem)
        VALUES ('{nome_remedio}', '{tarja}', {dosagem});
    """
    cur.execute(query)

# Função para inserir dados fictícios na tabela Consulta
def insert_consulta():
    codigo_con = fake.unique.random_int(min=1000, max=9999)
    data = fake.date_this_year()
    horario = fake.time()
    query = f"""
        INSERT INTO Consulta (codigo_con, data, horario)
        VALUES ({codigo_con}, '{data}', '{horario}');
    """
    cur.execute(query)

# Função para inserir dados fictícios na tabela Admin
def insert_admin():
    adm_id = fake.unique.random_int(min=1000, max=9999)
    username = fake.user_name()
    senha = fake.password(length=10)
    query = f"""
        INSERT INTO Admin (adm_id, "username", senha)
        VALUES ({adm_id}, '{username}', '{senha}');
    """
    cur.execute(query)

# Gerando e inserindo dados fictícios (50 registros para cada tabela)
for _ in range(50):  
    insert_paciente()
    insert_medico()
    insert_pedido_exame()
    insert_exame()
    insert_agend_exame()
    insert_receita()
    insert_remedio()
    insert_consulta()
    insert_admin()

conn.commit()

cur.close()
conn.close()

print("50 dados inseridos com sucesso em cada tabela!")

