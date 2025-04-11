# dio-modelo-conceitual-oficina
## este projeto trata do modelo lógico de um sistema de controle e gerenciamento de execução de ordens de serviço (OS) para uma oficina mecânica.
  - O sistema tem como objetivo otimizar o fluxo de trabalho, desde o momento em que o cliente leva o veículo para revisão ou conserto, passando pela identificação dos serviços, orçamento, execução e finalização da OS. A implementação de um sistema como este busca melhorar a organização da oficina, facilitar o acompanhamento do status dos serviços, agilizar a comunicação com os clientes e permitir um controle financeiro mais eficiente.

### Descrição do Projeto Lógico:
  - O script SQL <> cria o modelo lógico do sistema de controle e gerenciamento de ordens de serviço para uma oficina mecânica no MySQL, refletindo o modelo conceitual fornecido:

### SGBD Escolhido: 
  - MySQL foi utilizado neste script, diferente do PostgreSQL usado no projeto anterior.

### Nomes de Tabelas e Colunas: 
  - Os nomes foram mantidos conforme o modelo conceitual.

### Tipos de Dados: 
  - Os tipos de dados foram escolhidos de acordo com a descrição dos atributos.
  - INT PRIMARY KEY AUTO_INCREMENT para chaves primárias auto-incrementáveis no MySQL.
  - VARCHAR(n) para strings de tamanho variável com um limite máximo de n caracteres.
  - TIMESTAMP DEFAULT CURRENT_TIMESTAMP para armazenar data e hora, com o valor padrão sendo a hora atual na criação do registro.
  - DATE para armazenar apenas a data.
  - DECIMAL(10, 2) para números decimais com 10 dígitos no total, sendo 2 após a vírgula.
  - ENUM(...) para listas predefinidas de valores.

### Chaves Primárias (PRIMARY KEY): 
  - Definidas para garantir a unicidade dos registros em cada tabela.

### Chaves Estrangeiras (FOREIGN KEY): 
  - Estabelecem os relacionamentos entre as tabelas, garantindo a integridade referencial. A sintaxe é FOREIGN KEY (coluna) REFERENCES tabela(coluna).

### Chaves Primárias Compostas: 
  - Nas tabelas de ligação (ITEM_SERVICO, ITEM_PECA, EQUIPE_OS), a chave primária é uma combinação das chaves estrangeiras das tabelas relacionadas.

### Constraint UNIQUE: 
  - Adicionada à coluna PLACA_VEICULO para garantir que cada veículo tenha uma placa única.

### Constraint NOT NULL: 
  - Aplicada a atributos que são considerados obrigatórios.

### Relacionamentos Muitos-para-Muitos: 
  - Implementados através das tabelas de ligação (ITEM_SERVICO, ITEM_PECA, EQUIPE_OS), que contêm as chaves primárias das tabelas que estão sendo relacionadas.
