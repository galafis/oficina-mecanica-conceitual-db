# 🔧 Esquema Conceitual de Banco de Dados — Oficina Mecânica

## 📋 Descrição do Projeto

Este repositório contém o **esquema conceitual** de um banco de dados para o cenário de uma **Oficina Mecânica**, desenvolvido como parte da **Formação SQL Database Specialist** da [DIO (Digital Innovation One)](https://www.dio.me/).

O objetivo é modelar, do zero, um sistema de **controle e gerenciamento de execução de ordens de serviço** em uma oficina mecânica, com base na narrativa fornecida no desafio de projeto.

---

## 📖 Narrativa do Cenário

O sistema deve representar o seguinte contexto:

- Clientes levam seus **veículos** à oficina mecânica para serem **consertados** ou para passarem por **revisões periódicas**.
- Cada veículo é designado a uma **equipe de mecânicos**, que identifica os serviços a serem executados e preenche uma **Ordem de Serviço (OS)** com data de entrega.
- A partir da OS, calcula-se o **valor de cada serviço**, consultando-se uma **tabela de referência de mão de obra**.
- O **valor de cada peça** utilizada também compõe a OS.
- O **cliente autoriza** a execução dos serviços.
- A mesma equipe **avalia e executa** os serviços.
- Os mecânicos possuem: **código**, **nome**, **endereço** e **especialidade**.
- Cada OS possui: **número**, **data de emissão**, **valor**, **status** e **data para conclusão** dos trabalhos.

---

## 🏗️ Esquema Conceitual — Entidades e Atributos

### 1. Cliente

| Atributo | Tipo | Descrição |
|---|---|---|
| `idCliente` | INT (PK) | Identificador único do cliente |
| `Nome` | VARCHAR | Nome completo do cliente |
| `CPF` | CHAR(11) | CPF do cliente (único) |
| `Telefone` | VARCHAR | Telefone de contato |
| `Email` | VARCHAR | Endereço de e-mail |
| `Endereco` | VARCHAR | Endereço completo |

### 2. Veículo

| Atributo | Tipo | Descrição |
|---|---|---|
| `idVeiculo` | INT (PK) | Identificador único do veículo |
| `idCliente` | INT (FK) | Referência ao proprietário (Cliente) |
| `Placa` | CHAR(7) | Placa do veículo (única) |
| `Modelo` | VARCHAR | Modelo do veículo |
| `Marca` | VARCHAR | Marca do veículo |
| `Ano` | INT | Ano de fabricação |
| `Cor` | VARCHAR | Cor do veículo |

### 3. Mecânico

| Atributo | Tipo | Descrição |
|---|---|---|
| `idMecanico` | INT (PK) | Código identificador do mecânico |
| `Nome` | VARCHAR | Nome completo |
| `Endereco` | VARCHAR | Endereço do mecânico |
| `Especialidade` | VARCHAR | Especialidade (motor, elétrica, funilaria, etc.) |

### 4. Equipe

| Atributo | Tipo | Descrição |
|---|---|---|
| `idEquipe` | INT (PK) | Identificador único da equipe |
| `NomeEquipe` | VARCHAR | Nome ou código da equipe |
| `AreaAtuacao` | VARCHAR | Área de atuação da equipe |

### 5. EquipeMecanico (Associativa — N:M)

| Atributo | Tipo | Descrição |
|---|---|---|
| `idEquipe` | INT (FK) | Referência à equipe |
| `idMecanico` | INT (FK) | Referência ao mecânico |

> **Nota:** Um mecânico pode fazer parte de mais de uma equipe, e cada equipe é composta por vários mecânicos.

### 6. OrdemDeServico (OS)

| Atributo | Tipo | Descrição |
|---|---|---|
| `idOS` | INT (PK) | Número da Ordem de Serviço |
| `idVeiculo` | INT (FK) | Referência ao veículo |
| `idEquipe` | INT (FK) | Referência à equipe responsável |
| `DataEmissao` | DATE | Data de emissão da OS |
| `DataConclusao` | DATE | Data prevista para conclusão |
| `DataEntrega` | DATE | Data efetiva de entrega |
| `ValorTotal` | DECIMAL | Valor total da OS (serviços + peças) |
| `Status` | ENUM | Status: Em Avaliação, Aprovada, Em Execução, Concluída, Cancelada |
| `AutorizacaoCliente` | BOOLEAN | Indica se o cliente autorizou a execução |
| `TipoServico` | ENUM | Tipo: Conserto ou Revisão |

### 7. Serviço

| Atributo | Tipo | Descrição |
|---|---|---|
| `idServico` | INT (PK) | Identificador único do serviço |
| `Descricao` | VARCHAR | Descrição do serviço |
| `ValorMaoDeObra` | DECIMAL | Valor de referência da mão de obra |

> **Nota:** Os valores de mão de obra são consultados a partir de uma **tabela de referência**, representada pela entidade `Serviço`.

### 8. ServicoOS (Associativa — N:M entre OS e Serviço)

| Atributo | Tipo | Descrição |
|---|---|---|
| `idOS` | INT (FK) | Referência à Ordem de Serviço |
| `idServico` | INT (FK) | Referência ao serviço executado |
| `Quantidade` | INT | Quantidade de vezes que o serviço foi executado |
| `SubtotalServico` | DECIMAL | Subtotal do serviço nesta OS |

### 9. Peça

| Atributo | Tipo | Descrição |
|---|---|---|
| `idPeca` | INT (PK) | Identificador único da peça |
| `Descricao` | VARCHAR | Descrição da peça |
| `ValorUnitario` | DECIMAL | Preço unitário da peça |

### 10. PecaOS (Associativa — N:M entre OS e Peça)

| Atributo | Tipo | Descrição |
|---|---|---|
| `idOS` | INT (FK) | Referência à Ordem de Serviço |
| `idPeca` | INT (FK) | Referência à peça utilizada |
| `Quantidade` | INT | Quantidade de peças utilizadas |
| `SubtotalPeca` | DECIMAL | Subtotal das peças nesta OS |

---

## 🔗 Relacionamentos

| Relacionamento | Cardinalidade | Descrição |
|---|---|---|
| Cliente → Veículo | 1:N | Um cliente pode possuir vários veículos |
| Veículo → OrdemDeServico | 1:N | Um veículo pode ter várias OS ao longo do tempo |
| Equipe → OrdemDeServico | 1:N | Uma equipe pode ser responsável por várias OS |
| Mecânico ↔ Equipe | N:M | Um mecânico pode pertencer a várias equipes (via EquipeMecanico) |
| OrdemDeServico ↔ Serviço | N:M | Uma OS pode conter vários serviços (via ServicoOS) |
| OrdemDeServico ↔ Peça | N:M | Uma OS pode utilizar várias peças (via PecaOS) |

---

## 📊 Diagrama de Relacionamentos (Visão Geral)

```
  ┌──────────┐       ┌───────────┐       ┌──────────────────┐
  │ CLIENTE  │ 1   N │  VEÍCULO  │ 1   N │ ORDEM DE SERVIÇO │
  │          ├───────┤           ├───────┤                  │
  │ Nome     │possui │ Placa     │ gera  │ DataEmissão      │
  │ CPF      │       │ Modelo    │       │ ValorTotal       │
  │ Telefone │       │ Marca     │       │ Status           │
  └──────────┘       └───────────┘       └────────┬─────────┘
                                                   │
                              ┌─────────────────────┼─────────────────────┐
                              │                     │                     │
                         N    │                N    │                1    │
                    ┌─────────┴───┐       ┌─────────┴───┐       ┌────────┴───┐
                    │  SERVIÇO_OS │       │   PEÇA_OS   │       │   EQUIPE   │
                    │  (N:M)      │       │   (N:M)     │       │            │
                    └──────┬──────┘       └──────┬──────┘       └──────┬─────┘
                           │                     │                     │
                      N    │                N    │                N    │  M
                    ┌──────┴──────┐       ┌──────┴──────┐       ┌──────┴──────────┐
                    │  SERVIÇO    │       │    PEÇA     │       │ EQUIPE_MECÂNICO │
                    │             │       │             │       │     (N:M)       │
                    │ Descrição   │       │ Descrição   │       └──────┬──────────┘
                    │ ValorMDO    │       │ ValorUnit.  │              │
                    └─────────────┘       └─────────────┘         N   │
                                                              ┌──────┴──────┐
                                                              │  MECÂNICO   │
                                                              │             │
                                                              │ Nome        │
                                                              │ Especialid. │
                                                              └─────────────┘
```

---

## 🔄 Fluxo de Operação

1. O **cliente** leva seu **veículo** à oficina (conserto ou revisão).
2. O veículo é designado a uma **equipe de mecânicos**.
3. A equipe **avalia** o veículo e preenche uma **Ordem de Serviço (OS)**.
4. A OS lista os **serviços necessários** (com valores da tabela de mão de obra) e as **peças** necessárias.
5. O **cliente autoriza** a execução.
6. A equipe **executa** os serviços.
7. A OS é atualizada com o **status** e a **data de conclusão**.

---

## 🛠️ Ferramentas Utilizadas

- **MySQL Workbench** — Modelagem do diagrama ER
- **Draw.io (diagrams.net)** — Ferramenta alternativa de design
- **GitHub** — Versionamento e documentação do projeto

---

## 🎓 Contexto Acadêmico

Este projeto foi desenvolvido como **Desafio de Projeto** da **Formação SQL Database Specialist** oferecida pela **DIO (Digital Innovation One)**. O desafio consiste em criar um esquema conceitual do zero, a partir de uma narrativa de negócio, aplicando os conceitos de:

- Modelagem Entidade-Relacionamento (ER)
- Identificação de entidades, atributos e relacionamentos
- Definição de cardinalidades e restrições
- Criação de entidades associativas para relacionamentos N:M

---

## 📝 Licença

Este projeto é de uso educacional e foi desenvolvido para fins de aprendizado na plataforma DIO.

---

> **Autor:** Gabriel Demetrios Lafis
> **Formação:** SQL Database Specialist — DIO
> **Data:** Fevereiro de 2026
