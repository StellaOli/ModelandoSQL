## Diagrama MR
```mermaid
erDiagram
     Paciente{
        int paciente_id PK
        char nome_p
        int data_nasc 
        char convenio
        }


     Medico{
       int medico_id PK
       int crm
       char nome_m 
       char especialidade
        }


      Pedido_Exame{
        int num_pedido PK
        int data 
        int horario
        char tipo_exame
        }


      Exame{
        char nome_exame PK
        int categoria
        int codigo_exame
        }


       Agend_Exame{
          int exame_id  PK
          int data
          int horario
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


        Admin{
           int adm_id  PK
           char user
           int senha
         }

        Paciente_Consulta{
           int paciente_id PK    
           int codigo_con PK    
         }

        Medico_Receita{
           int receita_id  PK
           int medico_id PK
        }


    Medico ||--o{ Medico_Receita: o
    Medico ||--|{ Pedido_Exame: o
    Medico ||--|{ Consulta: o
    Medico ||--|{ Agend_Exame: o

    Paciente ||--|{ Agend_Exame : o
    Paciente ||--|{ Receita : o
    Paciente ||--|{ Paciente_Consulta : o

    Receita ||--|{ Medico_Receita: o
     
    Consulta }|--|| Paciente_Consulta : o
    
    Remedio }|--|{ Receita : o
    Exame }|--|{ Pedido_Exame : o
    Admin ||--|{ Agend_Exame : o
    


```
