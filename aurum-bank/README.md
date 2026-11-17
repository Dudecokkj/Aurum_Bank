# Aurum Bank — Sistema Bancário

Projeto A3 da disciplina de **Banco de Dados** · SGBD: **PostgreSQL**.
Professores: Dinho e Wilson · Instituição: São Judas Tadeu · Semestre: 4°

O **Aurum Bank** é um banco digital fictício com contas correntes e poupança (inclusive conjuntas), extrato de movimentações, cartão de crédito e empréstimos parcelados.

## Integrantes

Bruno de Oliveira Lemos RA: 312520604

Eduardo da Silva Jorge RA: 312510768

Gustavo Souza Silva RA: 312512250

Marcos Vinicius Sobrinho Silva RA: 312510533

Wagner Souza dos Santos Júnior RA: 312510823


## O que o projeto cobre

1. Modelo conceitual (DER)
2. Modelo lógico normalizado (até a 3FN)
3. Script DDL reexecutável
4. Carga de dados e manipulação (DML), incluindo cenário de concorrência
5. Consultas (DQL I e II), subconsultas, views, DCL e TCL

## Modelo de dados

12 tabelas: `agencia`, `cliente`, `conta`, `conta_corrente`, `conta_poupanca`, `conta_titular`, `categoria_transacao`, `transacao`, `cartao`, `compra_cartao`, `emprestimo` e `parcela_emprestimo`.

Padrões aplicados:

- **Especialização:** `conta` é o supertipo; `conta_corrente` e `conta_poupanca` são subtipos que reaproveitam a chave primária.
- **Relacionamento N:N:** cliente ↔ conta (`conta_titular`), que permite conta conjunta.
- **Tabela de eventos:** `transacao` registra cada movimentação e, nas transferências, referencia conta de origem e de destino.
- **Integridade:** `PRIMARY KEY`, `UNIQUE` (e-mail, CPF, número da conta), `CHECK` (valor > 0, status e tipos válidos, transferência exige destino), `DEFAULT`, `ON DELETE CASCADE` e `ON DELETE RESTRICT` (conta com extrato e cliente com empréstimo não podem ser apagados).

O diagrama e o dicionário de dados completos estão em [`docs/`](docs/Projeto_A3_Aurum_Bank.docx).

## Estrutura do repositório

```
aurum-bank/
├── docs/
│   ├── Projeto_A3_Aurum_Bank.docx          # relatório completo (DER, modelo lógico, dicionário, consultas)
│   └── der.png                             # diagrama entidade-relacionamento
├── sql/
│   ├── 01_script_ddl.sql                   # cria todas as tabelas (reexecutável)
│   ├── 01b_carga_dados.sql                 # dados fictícios (reexecutável)
│   ├── 02_operacoes_dml.sql                # INSERT, UPDATE, DELETE e transação com FOR UPDATE
│   ├── 03_consultas_dql.sql                # 14 consultas simples
│   ├── 04_consultas_dql2.sql               # 22 consultas: JOIN, GROUP BY, HAVING, agregações
│   └── 05_subconsultas_views_dcl_tcl.sql   # 10 subconsultas, 2 views, papel com GRANT/REVOKE, 7 passos de TCL
├── .gitignore
└── README.md
```

## Como executar

Pré-requisito: PostgreSQL instalado.

```bash
createdb aurum
psql -d aurum -f sql/01_script_ddl.sql
psql -d aurum -f sql/01b_carga_dados.sql
psql -d aurum -f sql/03_consultas_dql.sql
psql -d aurum -f sql/04_consultas_dql2.sql
psql -d aurum -f sql/05_subconsultas_views_dcl_tcl.sql
```

O `02_operacoes_dml.sql` **altera** os dados; para repetir o teste do zero, rode `01` e `01b` de novo.
Pelo **pgAdmin**: crie o banco `aurum`, abra o Query Tool e execute os arquivos na ordem acima.

> Os dados são fictícios, sem dados pessoais reais (LGPD). Os saldos das contas foram calculados a partir do extrato.
