# dio-modelo-conceitual-oficina
## este projeto trata da modelagem conceitual de um sistema de controle e gerenciamento de execução de ordens de serviço (OS) para uma oficina mecânica.
  - O sistema tem como objetivo otimizar o fluxo de trabalho, desde o momento em que o cliente leva o veículo para revisão ou conserto, passando pela identificação dos serviços, orçamento, execução e finalização da OS. A implementação de um sistema como este busca melhorar a organização da oficina, facilitar o acompanhamento do status dos serviços, agilizar a comunicação com os clientes e permitir um controle financeiro mais eficiente.

### Descrição do Projeto Conceitual
  - O esquema conceitual para o sistema de controle e gerenciamento de ordens de serviço em uma oficina mecânica é composto pelas seguintes entidades, atributos e relacionamentos:

**Entidades:**

* **CLIENTE:** Representa os proprietários dos veículos que utilizam os serviços da oficina.
    * Atributos:
        * `ID_CLIENTE` (PK): Identificador único do cliente.
        * `NOME_CLIENTE`: Nome completo do cliente.
        * `ENDERECO_CLIENTE`: Endereço do cliente.
        * `TELEFONE_CLIENTE`: Telefone de contato do cliente.
        * `EMAIL_CLIENTE`: Endereço de e-mail do cliente (opcional).

* **VEICULO:** Representa os veículos que são levados à oficina para serviços.
    * Atributos:
        * `ID_VEICULO` (PK): Identificador único do veículo.
        * `PLACA_VEICULO`: Placa do veículo.
        * `MODELO_VEICULO`: Modelo do veículo.
        * `MARCA_VEICULO`: Marca do veículo.
        * `ANO_VEICULO`: Ano de fabricação do veículo.
        * `ID_CLIENTE` (FK): Chave estrangeira referenciando a entidade CLIENTE.

* **ORDEM_SERVICO:** Representa o documento que formaliza a solicitação e execução dos serviços no veículo.
    * Atributos:
        * `NUMERO_OS` (PK): Número único da ordem de serviço.
        * `DATA_EMISSAO_OS`: Data em que a ordem de serviço foi emitida.
        * `DATA_ENTREGA_PREVISTA_OS`: Data prevista para a conclusão dos serviços.
        * `VALOR_TOTAL_OS`: Valor total da ordem de serviço.
        * `STATUS_OS`: Status atual da ordem de serviço (ex: Em aberto, Em execução, Concluída, Cancelada).
        * `ID_VEICULO` (FK): Chave estrangeira referenciando a entidade VEICULO.

* **MECANICO:** Representa os profissionais responsáveis pela avaliação e execução dos serviços nos veículos.
    * Atributos:
        * `CODIGO_MECANICO` (PK): Código único do mecânico.
        * `NOME_MECANICO`: Nome completo do mecânico.
        * `ENDERECO_MECANICO`: Endereço do mecânico.
        * `ESPECIALIDADE_MECANICO`: Especialidade do mecânico (ex: Motor, Elétrica, Freios).

* **SERVICO:** Representa os diferentes tipos de serviços que podem ser realizados na oficina.
    * Atributos:
        * `ID_SERVICO` (PK): Identificador único do serviço.
        * `DESCRICAO_SERVICO`: Descrição detalhada do serviço.
        * `VALOR_MO_SERVICO`: Valor da mão de obra para este serviço (consultado na tabela de referência).

* **PECA:** Representa as peças que podem ser utilizadas na execução dos serviços.
    * Atributos:
        * `ID_PECA` (PK): Identificador único da peça.
        * `NOME_PECA`: Nome da peça.
        * `DESCRICAO_PECA`: Descrição detalhada da peça.
        * `VALOR_UNITARIO_PECA`: Valor unitário da peça.
        * `QUANTIDADE_ESTOQUE_PECA`: Quantidade disponível em estoque (adicionado para melhor contexto).

**Relacionamentos:**

* Um `CLIENTE` possui um ou muitos `VEICULO`. (Um-para-muitos)
* Um `VEICULO` está associado a uma `ORDEM_SERVICO`. (Um-para-um)
* Uma `ORDEM_SERVICO` pode envolver um ou muitos `SERVICO`. (Muitos-para-muitos) - Relacionamento intermediário `ITEM_SERVICO`.
* Uma `ORDEM_SERVICO` pode utilizar uma ou muitas `PECA`. (Muitos-para-muitos) - Relacionamento intermediário `ITEM_PECA`.
* Uma `ORDEM_SERVICO` é designada a uma ou muitas `MECANICO` (para o trabalho em equipe). (Muitos-para-muitos) - Relacionamento intermediário `EQUIPE_OS`.
* Um `MECANICO` pode ser designado a uma ou muitas `ORDEM_SERVICO`. (Muitos-para-muitos) - Relacionamento intermediário `EQUIPE_OS`.

**Entidades de Ligação (para relacionamentos muitos-para-muitos):**

* **ITEM_SERVICO:** Associa uma ordem de serviço a um serviço específico.
    * Atributos:
        * `NUMERO_OS` (FK): Chave estrangeira referenciando a entidade ORDEM\_SERVICO.
        * `ID_SERVICO` (FK): Chave estrangeira referenciando a entidade SERVICO.
        * `QUANTIDADE_ITEM_SERVICO`: Quantidade deste serviço específico na OS (geralmente 1, mas pode variar).
        * `VALOR_UNITARIO_ITEM_SERVICO`: Valor unitário cobrado pelo serviço nesta OS (pode variar do valor de referência).
        * `SUBTOTAL_ITEM_SERVICO`: Valor total deste item de serviço na OS.
    * Chave Primária Composta: (`NUMERO_OS`, `ID_SERVICO`)

* **ITEM_PECA:** Associa uma ordem de serviço a uma peça utilizada.
    * Atributos:
        * `NUMERO_OS` (FK): Chave estrangeira referenciando a entidade ORDEM\_SERVICO.
        * `ID_PECA` (FK): Chave estrangeira referenciando a entidade PECA.
        * `QUANTIDADE_ITEM_PECA`: Quantidade da peça utilizada na OS.
        * `VALOR_UNITARIO_ITEM_PECA`: Valor unitário da peça cobrado nesta OS (pode ter margem).
        * `SUBTOTAL_ITEM_PECA`: Valor total deste item de peça na OS.
    * Chave Primária Composta: (`NUMERO_OS`, `ID_PECA`)

* **EQUIPE_OS:** Associa uma ordem de serviço a um ou mais mecânicos responsáveis.
    * Atributos:
        * `NUMERO_OS` (FK): Chave estrangeira referenciando a entidade ORDEM\_SERVICO.
        * `CODIGO_MECANICO` (FK): Chave estrangeira referenciando a entidade MECANICO.
    * Chave Primária Composta: (`NUMERO_OS`, `CODIGO_MECANICO`)

**Observações e Refinamentos:**

* A entidade `TABELA_REFERENCIA_MO` mencionada na narrativa foi implicitamente modelada dentro da entidade `SERVICO` através do atributo `VALOR_MO_SERVICO`. Em um sistema mais complexo, poderia ser uma entidade separada com critérios de precificação mais elaborados.
* O status da OS permite rastrear o progresso do serviço.
* A inclusão da entidade `PECA` e seu relacionamento com `ORDEM_SERVICO` complementa o processo de orçamentação e execução dos serviços.
* A entidade `EQUIPE_OS` permite registrar a equipe de mecânicos responsável por cada ordem de serviço, atendendo ao ponto VI da narrativa.
* Atributos como `TELEFONE_CLIENTE` e `EMAIL_CLIENTE` foram adicionados à entidade `CLIENTE` por serem informações de contato comuns e úteis em um sistema de gerenciamento.
* O atributo `QUANTIDADE_ESTOQUE_PECA` foi adicionado à entidade `PECA` para fornecer um contexto mais completo de gerenciamento da oficina.
