# Project Overview: Como funciona este ERP

## 1. Introdução

Este documento detalha o propósito, a arquitetura e a direção estratégica do nosso ERP Imobiliário. Ele serve como base para orientar o desenvolvimento e garantir o alinhamento técnico e de negócios.

* **O que é o ERP?** Uma plataforma completa de gestão voltada para o mercado imobiliário.
* **Por que ele está sendo desenvolvido?** Para modernizar e centralizar as operações de imobiliárias em um único ecossistema seguro e escalável.
* **Qual problema resolve?** A fragmentação de sistemas, a dificuldade de escalar operações, processos manuais e a falta de integração entre setores (vendas, locação, financeiro).
* **Qual sua proposta de valor?** Oferecer uma solução *all-in-one* baseada na nuvem que automatiza fluxos de ponta a ponta com alta disponibilidade e integração de Inteligência Artificial.

---

## 2. Visão Geral

O projeto é uma plataforma **SaaS Enterprise** desenvolvida para atender de forma unificada aos principais pilares operacionais de uma imobiliária:

* Venda
* Locação
* CRM (Gestão de Relacionamento)
* Financeiro
* Contratos
* Atendimento
* Integrações com portais e serviços
* Inteligência Artificial

---

## 3. Objetivos

### Objetivos de Negócio

* Centralizar a gestão imobiliária em um ambiente único.
* Automatizar processos manuais e burocráticos.
* Escalar operações com facilidade e segurança.

### Objetivos Técnicos

* Manter alta disponibilidade (High Availability).
* Operar em modelo Multi-Tenant (Múltiplas empresas na mesma base, com dados isolados).
* Seguir o conceito Cloud Native.
* Adotar a abordagem API First.
* Garantir uma Arquitetura Modular e testável.

---

## 4. Princípios Norteadores

Decisões fundamentais que orientam todo o desenvolvimento e ajudam a equipe a tomar decisões arquiteturais e de código:

* **DDD (Domain-Driven Design):** Utilizado como base da modelagem do sistema.
* **Clean Architecture:** Foco na separação clara de responsabilidades.
* **API First:** APIs projetadas como o principal canal de comunicação do produto.
* **Security by Design:** Segurança pensada desde a concepção de cada funcionalidade.
* **Testabilidade:** Código escrito para ser facilmente testável.
* **Observabilidade:** Monitoramento constante da saúde e performance da aplicação.
* **Multi-tenancy:** Capacidade multiempresa implementada desde a fundação.

---

## 5. Stack Tecnológica

| Camada | Tecnologia |
| :--- | :--- |
| **Backend** | .NET 9 |
| **Frontend** | React + TypeScript |
| **Banco de Dados** | PostgreSQL |
| **Cache** | Redis |
| **Mensageria** | RabbitMQ |
| **Infraestrutura** | Docker |
| **Proxy** | NGINX |
| **Hospedagem** | Hostinger VPS |
| **Deploy** | EasyPanel |
| **DNS** | Cloudflare |

---

## 6. Arquitetura

O fluxo de requisição e responsabilidade obedece à seguinte hierarquia estrutural:

```text
Usuário
   ↓
Frontend
   ↓
API
   ↓
Application
   ↓
Domain
   ↓
Framework
   ↓
Infrastructure
   ↓
PostgreSQL

```

---

## 7. Estrutura e Organização do Projeto

O ecossistema é dividido estrategicamente em dois repositórios principais:

### Enterprise Architecture

Responsável pelas definições base, padronização e documentação.

* Governança
* Arquitetura
* DDD
* Framework Base
* Documentação

### Platform

Responsável pela implementação real do produto.

* Código-fonte
* Backend e API
* Frontend
* Banco de Dados
* Infraestrutura
* Testes Automatizados

---

## 8. Domínios

| Domínio | Objetivo |
| --- | --- |
| **Property** | Gestão de imóveis (cadastro, captação, mídia) |
| **Customer** | Gestão de clientes (compradores, locatários) |
| **Owner** | Gestão de proprietários |
| **Broker** | Gestão de corretores e parceiros |
| **Contracts** | Gestão de contratos (geração, assinaturas) |
| **CRM** | Gestão comercial, funil de vendas e leads |
| **Finance** | Gestão financeira, faturamento e repasses |
| **Identity** | Autenticação, autorização e segurança |
| **Tenant** | Gestão multiempresa e isolamento de dados |

---

## 9. Framework Próprio

O projeto conta com um framework base interno para encapsular padrões repetitivos, contendo:

* Result Pattern
* Repository Pattern
* Domain Events
* Specification Pattern
* Unit of Work
* Auditoria de Dados
* Isolamento Multi-Tenant
* Políticas de Segurança

---

## 10. Critérios de Sucesso

Indicadores claros para avaliar a saúde técnica e a entrega de valor do projeto:

* Todos os módulos seguem rigorosamente o `ERP.Framework`.
* Cobertura mínima de testes garantida nas camadas core.
* APIs totalmente documentadas com OpenAPI (Swagger).
* Arquitetura validada continuamente por testes de dependência (ArchUnit).
* Documentação técnica sempre atualizada em conjunto com o código-fonte.

---

## 11. Estado Atual

* **Arquitetura:** ██████████ 100%
* **Documentação:** ███████░░░ 70%
* **Framework:** ░░░░░░░░░░ 0%
* **Backend:** ░░░░░░░░░░ 0%
* **Frontend:** ░░░░░░░░░░ 0%

---

## 12. Roadmap

O cronograma de execução do projeto seguirá este fluxo lógico:

1. Definição da Arquitetura
2. Construção do Framework Base
3. Desenvolvimento do Backend
4. Desenvolvimento do Frontend
5. Integrações Externas
6. Implementação de IA
7. Lançamento em Produção
