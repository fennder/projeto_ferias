## Crie o Banco de Dados para Fluxo de Solicitação de Férias e Pagamento

Este modelo de banco de dados visa otimizar o fluxo de solicitação de férias, desde a submissão pelo funcionário até o pagamento pelo financeiro, além de gerar uma visão geral dos dias de férias por funcionário.

**Tabelas:**

1.  **Funcionarios:**
    * `funcionario_id` (INT, PRIMARY KEY)
    * `nome` (VARCHAR)
    * `departamento` (VARCHAR)
    * `cargo` (VARCHAR)
    * `data_admissao` (DATE)

2.  **SolicitacoesFerias:**
    * `solicitacao_id` (INT, PRIMARY KEY)
    * `funcionario_id` (INT, FOREIGN KEY references Funcionarios)
    * `data_inicio` (DATE)
    * `data_fim` (DATE)
    * `dias_solicitados` (INT)
    * `status` (VARCHAR) -- (Ex: "Pendente", "Aprovado", "Negado", "Pago")
    * `data_solicitacao` (DATE)
    * `data_aprovacao` (DATE)
    * `observacoes` (TEXT)

3.  **Aprovacoes:**
    * `aprovacao_id` (INT, PRIMARY KEY)
    * `solicitacao_id` (INT, FOREIGN KEY references SolicitacoesFerias)
    * `aprovador_id` (INT, FOREIGN KEY references Funcionarios)
    * `data_aprovacao` (DATE)
    * `status` (VARCHAR) -- (Ex: "Aprovado", "Negado")
    * `observacoes` (TEXT)

4.  **Pagamentos:**
    * `pagamento_id` (INT, PRIMARY KEY)
    * `solicitacao_id` (INT, FOREIGN KEY references SolicitacoesFerias)
    * `valor_pago` (DECIMAL)
    * `data_pagamento` (DATE)
    * `metodo_pagamento` (VARCHAR)

**Visão (View): Dias de Férias por Funcionário**

```sql
CREATE VIEW vw_dias_ferias_funcionario AS
SELECT
    f.funcionario_id,
    f.nome,
    SUM(sf.dias_solicitados) AS total_dias_ferias
FROM
    Funcionarios f
JOIN
    SolicitacoesFerias sf ON f.funcionario_id = sf.funcionario_id
WHERE
    sf.status = 'Aprovado' -- ou 'Pago', dependendo da necessidade
GROUP BY
    f.funcionario_id, f.nome;
```

**Explicação:**

* **Tabelas:**
    * `Funcionarios`: Armazena informações dos funcionários.
    * `SolicitacoesFerias`: Registra as solicitações de férias, incluindo datas, status e observações.
    * `Aprovacoes`: Rastreia as aprovações das solicitações, indicando quem aprovou e quando.
    * `Pagamentos`: Registra os pagamentos das férias, incluindo valor e método.
* **Visão (View):**
    * `vw_dias_ferias_funcionario`: Calcula o total de dias de férias aprovados (ou pagos) para cada funcionário.

**Observações:**

* Este modelo pode ser adaptado para incluir mais detalhes, como políticas de férias, limites de dias por ano, etc.
* A coluna `status` nas tabelas `SolicitacoesFerias` e `Aprovacoes` pode ser substituída por uma tabela de lookup para maior controle.
* A visão `vw_dias_ferias_funcionario` pode ser modificada para incluir filtros por departamento, período, etc.
* A tabela `Pagamentos` pode ser integrada com um sistema financeiro externo, se necessário.


**GERAR O RELATÓRIO CONFORME ESTÁ DESCRITO NA VIEW COM PELO MENOS 50 REGISTROS**
