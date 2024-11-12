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
     - `exame_id`
     - `data`
     - `horario`
     - `medico_id`
