# Modelo Entidade Relacional
## Entidades 

  - **Paciente**
      - `paciente_id` (PK)
      - `nome_p`
      - `data_nasc`
      - `convenio`


 - **Medico**
     - `medico_id` (PK)
     - `crm`
     - `nome_m`
     - `especialidade`


 - **Pedido_Exame**
     - `num_pedido` (PK)
     - `data`
     - `horario`
     - `tipo_exame`

  - **Exame**
      - `nome_exame` (PK)
      - `categoria`
      - `codigo_exame`
  
  - **Agend_Exame**
      - `exame_id` (PK)
      -  `data`
      -  `horário`
  
  - **Receita**
      -  `receita_id` (PK)
      -  `data`
      -  `tipo_receita`

  - **Remedio**
      - `nome_remedio` (PK)
      -  `tarja`
      -  `dosagem`
        
  - **Consulta**
      -  `codigo_con` (PK)
      -  `data`
      -  `horario`
   
  - **Admin**
      - `adm_id` (PK)
      - `user`
      - `senha`

        
## Diagrama MER

```mermaid

---
title: Modelo Entidade Relacional
---

classDiagram
    class Paciente{
        paciente_id PK
        nome_p char(50)
        data_nasc int(8)
        convenio char (50)
    }

    class Medico{
       medico_id PK
       crm int(6)
       nome_m char(50)
       especialidade char(30)
    }

     class Pedido_Exame{
        num_pedido PK
        data int(8)
        horario int(6)
        tipo_exame char(30)
    }
      class Exame{
       nome_exame PK
       categoria int(2)
       codigo_exame int(7)
    }
      class Agend_Exame{
          exame_id  PK
          data int(8)
          horario int(6)
    }
      class Receita{
         receita_id  PK
         data int(8)
         tipo_receita char(7)
    }
      class Remedio{
         nome_remedio  PK
         tarja char(7)
         dosagem float(4)
     }   
       class Consulta{
            codigo_con PK
            data int(8)
            horario int(6)
     }
        class Admin{
           adm_id  PK
           user  char(15)
           senha  int(8)
    }
     

    Receita "n" --> "n" Remedio : Contém
    Remedio "n" --> "n" Receita : Aparece

    Medico "1" --> "n" Pedido_Exame : Faz

    Paciente "1" --> "n" Agend_Exame : e atendido
    Medico "1" --> "n" Agend_Exame : Atende

    Paciente "1" --> "n" Consulta : e atendido
    Medico "1" --> "n" Consulta  : Atende
    
    Admin "1" --> "n" Agend_Exame : Agenda 
    Paciente "1" --> "n" Receita : Recebe
    Medico "1" --> "n" Receita : Preescreve
    

    Exame "n" --> "n" Pedido_Exame : Aparece

    Pedido_Exame "1" --> "1" Agend_Exame : Aparece

    Admin "1" --> "n" Consulta : Agenda
   
    








``` 
