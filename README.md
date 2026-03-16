# Esquema Conceitual de Banco de Dados - Oficina Mecanica

<div align="center">

![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Database](https://img.shields.io/badge/Database-Modeling-orange?style=for-the-badge&logo=databricks&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**[PT-BR](#sobre-o-projeto) | [English](#about-the-project)**

</div>

---

<a name="sobre-o-projeto"></a>

## Sobre o Projeto

> Desafio de projeto da **Formacao SQL Database Specialist** -- [DIO (Digital Innovation One)](https://www.dio.me/)

Este repositorio contem o esquema conceitual de um banco de dados para o cenario de uma Oficina Mecanica, modelando do zero um sistema de controle e gerenciamento de ordens de servico (OS), incluindo clientes, veiculos, equipes de mecanicos, servicos e pecas.

---

## Fluxo de Operacao

```mermaid
flowchart LR
    A[Cliente\\nVeiculo] --> B[Equipe de\\nMecanicos]
    B --> C[Ordem de\\nServico]
    C --> D[Servicos +\\nPecas]
    D --> E[Autorizacao\\nCliente]
    E --> F[Execucao e\\nEntrega]

    style A fill:#4479A1,color:#fff,stroke:#2c5f8a
    style C fill:#e67e22,color:#fff,stroke:#cc6a1e
    style F fill:#27ae60,color:#fff,stroke:#1e8449
```

---

## Entidades Principais

- **Cliente**: Dados cadastrais dos proprietarios
- **Veiculo**: Veiculos com placa, modelo, marca e ano (1:N com Cliente)
- **Mecanico**: Profissionais com codigo, nome e especialidade
- **Equipe**: Equipes de mecanicos (N:M com Mecanico)
- **OrdemDeServico**: OS com numero, datas, valor, status e autorizacao
- **Servico**: Tabela de referencia de servicos e valores de mao de obra
- **Peca**: Catalogo de pecas com valores unitarios

## Relacionamentos N:M

- Equipe <-> Mecanico (via EquipeMecanico)
- OrdemDeServico <-> Servico (via ServicoOS)
- OrdemDeServico <-> Peca (via PecaOS)

## Aplicacao na Industria

A modelagem conceitual garante que todos os requisitos de negocio sejam capturados antes da implementacao, reduzindo retrabalho e custos em projetos de banco de dados.

---

<a name="about-the-project"></a>

## English

### About the Project

> Project challenge from the **SQL Database Specialist** program -- [DIO](https://www.dio.me/)

This repository contains the conceptual database schema for a Car Repair Shop scenario, modeling from scratch a service order (SO) control and management system, including customers, vehicles, mechanic teams, services, and parts.

### Key Entities

- Client, Vehicle, Mechanic, Team, ServiceOrder, Service, Part
- N:M relationships via associative entities (TeamMechanic, ServiceSO, PartSO)

---

## Licenca | License

Este projeto esta licenciado sob a [Licenca MIT](LICENSE). | This project is licensed under the [MIT License](LICENSE).

---

Developed by [Gabriel Demetrios Lafis](https://github.com/galafis)
