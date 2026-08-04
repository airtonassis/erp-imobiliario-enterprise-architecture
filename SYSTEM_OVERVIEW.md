# System Overview

## Execution Context

A **Business Management Platform** opera como um ecossistema vivo e interconectado. Este documento ilustra como as diferentes partes da plataforma — desde os componentes de infraestrutura até os aplicativos de interface do usuário — se comunicam e operam em conjunto para entregar valor aos produtos verticais. A plataforma não é um sistema único, mas uma constelação de microserviços, agentes de IA e interfaces orquestrados em tempo real.

Um usuário autenticado realiza uma operação.

↓

API Gateway

↓

Tenant Resolution

↓

Authorization

↓

Application Layer

↓

Domain

↓

Persistence

↓

Event Bus

↓

Background Workers

↓

AI

↓

Observability

## Componentes

Os blocos de construção estruturais que formam a topologia da plataforma:
   **Aplicações Cliente:** Micro-frontends web e aplicativos móveis que servem como ponto de contato para proprietários, inquilinos e corretores.
   **API Gateway:** A porta de entrada unificada. Responsável por roteamento, terminação SSL, bloqueio de ameaças (WAF) e validação inicial de identidade e *Tenant*.
   **Barramento de Eventos (Event Bus):** O sistema nervoso central (RabbitMQ/Kafka) que roteia mensagens de domínio entre domínios isolados.
   **Bancos de Dados:** Instâncias isoladas lógicas ou físicas (PostgreSQL, MongoDB) divididas por Bounded Contexts.
   **AI Engine:** Camada de orquestração de LLMs e banco vetorial responsável pelo RAG (Retrieval-Augmented Generation) e execução de Agentes.

## Serviços

Os executores autônomos dentro da plataforma. Cada serviço é dono de seus próprios dados e regras de negócio:
   **Serviços Compartilhados (Capabilities):** `CrmService`, `FinanceService`, `DocumentService`, `NotificationService`, `WorkflowService`.
   **Serviços Verticais:** `RealEstateService` (Catálogo de imóveis, gestão de locação, contratos).
   **Serviços de Background:** *Workers* assíncronos que processam filas, geram relatórios pesados e treinam índices de IA fora do fluxo HTTP principal.

## Framework

O `business-platform-framework` atua como a cola padronizada entre os serviços. Quando a plataforma está em execução, o Framework garante que:
   Toda requisição HTTP seja interceptada para extração do `TenantId`.
   Toda falha de negócio retorne um *Result Pattern* padronizado em vez de quebrar a aplicação com uma exceção genérica.
   Todo log gerado por qualquer serviço seja formatado da mesma maneira para ser facilmente ingerido pelo nosso stack de observabilidade.

## Core

A infraestrutura invisível que sustenta a operação diária. Durante a execução do sistema, o Core:
   Valida se o usuário que fez a requisição pertence àquele *Tenant* e se o *Tenant* está ativo.
   Gera as trilhas de auditoria (Audit Logs) para qualquer alteração de dados sensíveis.
   Gerencia os segredos e configurações dinâmicas que os serviços consomem na inicialização.

## Produtos

A face visível da plataforma. O módulo **Real Estate**, por exemplo, não é um banco de dados monolítico, mas uma composição:
   A interface do usuário do Real Estate consome dados do `RealEstateService` para mostrar os imóveis.
   Consome o `CrmService` para listar os inquilinos.
   Consome o `FinanceService` para exibir o painel de faturamento da imobiliária.
Tudo isso é consolidado no Front-end ou via um padrão BFF (Backend-For-Frontend) para o usuário final.

## Fluxo Geral

Para entender a plataforma funcionando, observe o fluxo de **"Assinatura de um Contrato de Locação"**:
 O usuário (Corretor) clica em "Assinar" na interface web.
 A requisição bate no **API Gateway**, que valida o Token JWT e o contexto do *Tenant*.
 O Gateway roteia a chamada para o **RealEstateService**.
 O `RealEstateService` valida as regras de domínio imobiliário, atualiza o status do contrato no banco de dados para "Assinado" e publica o evento `LeaseContractSigned` no **Event Bus**.
 O `RealEstateService` responde "Sucesso" ao usuário quase instantaneamente.
 **Em background (Assíncrono):**
  O **FinanceService** ouve o evento, calcula o repasse da imobiliária e gera as faturas do aluguel.
  O **DocumentService** ouve o evento, gera o PDF em definitivo e arquiva no S3.
  O **NotificationService** ouve o evento e envia um e-mail de boas-vindas ao inquilino.
  O **AI Engine** atualiza o contexto vetorial do locatário para que o chatbot saiba que ele agora tem um contrato ativo.

## Comunicação

   **Externa (Cliente -> Plataforma):** Síncrona, via REST (HTTPS) através do API Gateway.
   **Interna (Serviço -> Serviço - Leitura):** Síncrona, via gRPC, para dados imediatos estritamente necessários.
   **Interna (Serviço -> Serviço - Mutação):** Assíncrona, via publicação e subscrição (Pub/Sub) no *Event Bus*. Nenhuma mudança de estado inter-domínios é feita de forma síncrona.

## Integrações

A plataforma se comunica com o mundo de forma isolada:
   **Webhooks de Entrada:** Recebemos retornos de pagamentos de bancos através de endpoints específicos que traduzem o layout do banco para eventos internos (Camada Anticorrupção).
   **APIs de Terceiros:** A IA e os processos de *background* acessam serviços externos (Bureaus de Crédito, Seguradoras) sempre utilizando circuit breakers e retentativas (*Retries*) para evitar que falhas externas derrubem a operação interna.

## Deploy

O sistema é continuamente entregue (CI/CD) para a nuvem.
   Cada repositório (Core, Framework, Produtos) possui sua própria esteira de build independente.
   Ao integrar código na branch principal, imagens Docker são geradas e enviadas para um registro (Container Registry).
   O Kubernetes orquestra a implantação das novas versões utilizando estratégias de *Rolling Update* (zero tempo de inatividade).

## Roadmap Técnico

A evolução de engenharia do ecossistema:
   **Fase 1 (Atual):** Documentação de arquitetura, estabelecimento das fundações e acordos (Sprint 001).
   **Fase 2:** Desenvolvimento e publicação do repositório `business-platform-framework` (Shared Kernel).
   **Fase 3:** Construção do *Platform Core* e seus serviços vitais (Identity, Tenant).
   **Fase 4:** Desenvolvimento MVP dos *Shared Capabilities* (CRM, Finance) e infraestrutura base de AI.
   **Fase 5:** Início do desenvolvimento da lógica vertical no repositório `business-platform-real-estate`.
  
## System Context

  Usuários

↓

Business Platform

↓

Parceiros

↓

Bancos

↓

ERP

↓

Marketplace

↓

Gov APIs

↓

LLMs

## Platform Lifecycle

Developer

↓

Commit

↓

CI

↓

Build

↓

Container

↓

Registry

↓

GitOps

↓

Kubernetes

↓

Observability

↓

AI Monitoring

## Failure Strategy

Se um serviço cair

↓

Circuit Breaker

↓

Retry

↓

Fallback

↓

DLQ

↓

Alerta

↓

Observabilidade

## Scalability Strategy

Frontend

↓

Horizontal

API

↓

Horizontal

RabbitMQ

↓

Cluster

Redis

↓

Cluster

PostgreSQL

↓

Read Replica

Mongo

↓

Sharding

## AI Runtime

Prompt

↓

Context

↓

RAG

↓

Vector Search

↓

Memory

↓

Tool Calling

↓

Business API

↓

Response

↓

Audit

## Operational Principles

Todo serviço deve ser:
Stateless
Containerizado
Observável
Versionado
Documentado
Testável
Multi Tenant
Seguro
Escalável
