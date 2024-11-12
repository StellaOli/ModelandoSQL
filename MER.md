## Entidades 

  - **Paciente**
      - `paciente_id` (PK)
      - `nome`
      - `data_nasc`
      - `convenio`


 - **Médico**
     - `medico_id` (PK)
     - `crm`
     - `nome`


 - **Agendamento_Exame**
     - `exame_id` (PK)
     - `data`
     - `horario`


  - **Tipo_Exame**
      - `nome_exame`
      -  ``
   
  - **Especialidade**
      - `codigo_esp` (PK)
      -  `nome_esp`
      -  `qtde_medicos`
      -  `medico_chefe`
   
  - **Consulta**
      - `codigo_con` (PK)
      -  `data`
      -  `horario`
      -   `medico`
      -   `paciente`
        
