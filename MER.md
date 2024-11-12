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


 - **Pedido_Exame**
     - `num_pedido` (PK)
     - `data`
     - `horario`
     - `tipo_exame`
     
  
  - **Agendamento_Exame**
      - `exame_id` (PK)
      -  `data`
      -  `horário`
  
  - **Especialidade**
      -  `codigo_esp` (PK)
      -  `nome_esp`
      -  `qtde_medicos`
      -  `medico_chefe`
   
  - **Consulta**
      -  `codigo_con` (PK)
      -  `data`
      -  `horario`
   
  - **Admin**
      - `adm_id` (PK)
      - `user`
      - `senha`

        
