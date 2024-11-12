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
     - `especialidade`


 - **Pedido_Exame**
     - `num_pedido` (PK)
     - `data`
     - `horario`
     - `tipo_exame`
     
  
  - **Exame**
      - `exame_id` (PK)
      -  `data`
      -  `horário`
  
  - **Receita**
      -  `receita_id` (PK)
      -  `data`
      -  `tipo_receita`
        
  - **Consulta**
      -  `codigo_con` (PK)
      -  `data`
      -  `horario`
   
  - **Admin**
      - `adm_id` (PK)
      - `user`
      - `senha`

        
