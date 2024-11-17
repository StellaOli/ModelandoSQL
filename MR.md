## Diagrama MR
```mermaid
erDiagram
       Paciente{
         int paciente_id PK
         char nome_p
         int data_nasc 
         char convenio
         int cpf
         int telefone 
        }

       Medico{
         int medico_id PK
         int crm
         char nome_m 
         char especialidade
        }

       Receita{
         int receita_id  PK
         int data
         char tipo_receita
        }

       Remedio{
         char nome_remedio  PK
         char tarja 
         float dosagem 
         }

       Consulta{
           int codigo_con PK
           int data
           int horario
         }

       Status_Consulta{
           int status_id PK 
           char descricao_status
         } 

        Admin{
           int adm_id  PK
           char user
           int senha
           char email  
         }

        Paciente_Consulta{
           int paciente_id PK    
           int codigo_con PK    
         }

        Paciente_Medico{
           int paciente_id PK    
           int medico_id PK    
         }

        Consulta_Medico{
           int codigo_con PK
           int medico_id PK
         }

        Receita_Consulta{
           int receita_id  PK
           int codigo_con PK
        }

        Receita_Remedio{
           int receita_id  PK
           char nome_remedio  PK
           int quantidade
        }


    Medico ||--o{ Receita: o
    Medico }|--|{ Paciente_Medico: o
    Medico ||--|{ Consulta_Medico: o

    Paciente }|--|{ Paciente_Medico : o
    Paciente ||--|{ Receita : o
    Paciente ||--|{ Paciente_Consulta : o

    Receita }|--|{ Receita_Remedio: o
    Receita }|--|| Receita_Consulta: o
     
    Consulta }|--|| Paciente_Consulta : o
    Consulta }|--|| Consulta_Medico : o
    Consulta ||--|{ Receita_Consulta : o
    Consulta }|--|| Status_COnsulta : o 
    
    Remedio }|--|{ Receita_Remedio : o
   
    Admin ||--o{ Consulta : o
    


```
