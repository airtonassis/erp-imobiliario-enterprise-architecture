# Platform Architecture

## Visão Geral

A arquitetura da **Business Management Platform** é desenhada como um ecossistema modular e extensível, operando no modelo *Hub & Spoke* (Núcleo e Raios). Em vez de construir aplicações verticais monolíticas do zero, desenvolvemos um Hub centralizado (*Core* e *Shared Capabilities*) onde múltiplos produtos (*Business Modules*) podem se plugar. Esta abordagem garante alta reutilização de código, isolamento de domínios (Domain-Driven Design) e a injeção nativa de Inteligência Artificial em todos os fluxos da plataforma.

## Arquitetura em Camadas

A plataforma é conceitualmente dividida em quatro grandes camadas lógicas, garantindo a separação de responsabilidades e o baixo acoplamento:

 **Developer Platform (O Como):** Frameworks, padrões e infraestrutura de desenvolvimento.
 **Platform Core (A Fundação):** Serviços técnicos indispensáveis para o funcionamento do sistema em nuvem.
 **Shared Capabilities (O Motor de Negócio):** Serviços de domínio genéricos e reutilizáveis.
 **Business Modules / Industry Solutions (O Produto):** As aplicações verticais (ex: Real Estate) que consomem as camadas inferiores para entregar valor direto ao mercado.

## Platform Core

O *Core* abriga os serviços de infraestrutura e governança da plataforma. Nenhum outro módulo funciona sem que o *Core* esteja operante. Ele engloba:
   **Identity & Access Management (IAM):** Autenticação de usuários, gestão de perfis, controle de acesso baseado em roles (RBAC) e permissões granulares.
   **Tenant Management:** Resolução e gestão do ciclo de vida dos inquilinos (clientes).
   **Security & Audit:** Rastreabilidade de ações (quem fez o quê, quando) e criptografia.
   **Configuration:** Gestão centralizada de parâmetros globais e configurações específicas por tenant.

## Shared Capabilities

Serviços de negócio transversais que fornecem capacidades prontas para qualquer produto da plataforma. Eles operam como *Bounded Contexts* independentes:
   **CRM:** Gestão unificada de Pessoas (Físicas e Jurídicas), Contatos e Interações.
   **Finance:** Motor financeiro central (Contas a Pagar/Receber, Faturamento, Conciliação).
   **Workflow:** Motor de processos e aprovações dinâmicas.
   **Documents & Files:** Armazenamento seguro (Blob Storage), geração de contratos e gestão de pastas.
   **Notifications:** Serviço centralizado de mensageria (E-mail, SMS, Push, Webhooks).
   **Analytics & BI:** Processamento e visualização de dados para relatórios e dashboards.

## Business Modules

As soluções de indústria (ex: *business-platform-real-estate*) são construídas como módulos fracamente acoplados que orquestram as *Shared Capabilities* para resolver problemas específicos de um nicho.
   Eles contêm **apenas** a regra de negócio específica do seu setor (ex: Contrato de Locação, Vistorias, Repasses).
   Não recriam serviços base; eles delegam ao *Core* (ex: O pagamento de um aluguel é enviado ao módulo *Finance* compartilhado).

## Framework

Para garantir consistência em todos os repositórios, a plataforma fornece um *Shared Kernel* (Framework Base) contendo:
   **Design Patterns:** Implementações padronizadas de *Result Pattern* (para controle de fluxo sem exceções), *Repository Pattern* e *Specification Pattern*.
   **Infraestrutura Cross-Cutting:** Injeção de dependência, tratamento global de erros e logging.
   **SDKs Internos:** Bibliotecas facilitadoras para comunicação entre módulos e serviços.

## AI Platform

A inteligência artificial atua transversalmente na arquitetura.
   **AI Agents:** Entidades autônomas que possuem acesso às APIs da plataforma para executar tarefas (ex: agendar vistorias, cobrar clientes).
   **AI Workflows:** Cadeias de execução onde a IA toma decisões de roteamento com base em dados de contexto.
   **AI Knowledge & Context (RAG):** Vetorização de dados do tenant (documentos, histórico do CRM) permitindo que os modelos gerativos respondam e atuem baseados na realidade específica daquele cliente, respeitando rigorosamente o isolamento do tenant.

## Event Driven

A comunicação entre domínios distintos (*Bounded Contexts*) prioriza a **arquitetura orientada a eventos**.
   Utilizamos um **Event Bus** (barramento de eventos de domínio e integração).
   Quando uma ação ocorre em um módulo (ex: `LeaseContractSigned` no Real Estate), um evento é publicado no barramento. Módulos interessados (como o *Finance* para gerar a cobrança e o *Notifications* para enviar um e-mail) reagem de forma assíncrona, garantindo baixo acoplamento e alta performance.

## Multi Tenant

A plataforma é *Multi-Tenant by Design*.
   O contexto do Tenant (`TenantId`) é inferido na borda da aplicação (via Token JWT ou Headers no API Gateway) e injetado automaticamente em todas as requisições, logs, eventos e queries de banco de dados.
   Utilizamos estratégias de isolamento lógico (dados de múltiplos tenants no mesmo banco, separados por `TenantId` via *Global Query Filters* no Framework) ou isolamento físico (bancos dedicados para tenants Enterprise), de forma transparente para as regras de negócio.

## APIs

   **API Gateway:** Ponto único de entrada (Single Point of Entry) para todas as interfaces clientes (Web, Mobile). Ele lida com roteamento, terminação SSL, *Rate Limiting* e validação preliminar de tokens.
   **API-First:** Todos os serviços expõem contratos claros (REST/GraphQL e OpenAPI/Swagger). Nenhuma lógica de negócio reside nas interfaces de usuário (Front-end).

## Integrações

A arquitetura prevê a necessidade de conversar com o mundo externo (Bancos, ERPs legados, Portais Imobiliários).
   Utilizamos o padrão de **Camada Anticorrupção (ACL)** para evitar que os modelos de domínio internos sejam contaminados pelos modelos de sistemas de terceiros.
   Suporte robusto a Webhooks de entrada e saída.

## Segurança

Adoção do modelo **Zero Trust**.
   Comunicação entre serviços internos também exige autenticação (Service-to-Service auth).
   Nenhum dado sensível ou PII (Personally Identifiable Information) trafega sem criptografia (em repouso e em trânsito).
   Auditoria automática em nível de banco de dados e de aplicação de todas as operações de mutação (Criação, Atualização, Deleção).

## Observabilidade

Visibilidade total sobre o comportamento da plataforma em produção.
   **Logs Estruturados:** Todos os logs carregam contexto essencial (`CorrelationId`, `TenantId`, `UserId`).
   **Tracing Distribuído:** Rastreamento do ciclo de vida de uma requisição desde o API Gateway até os bancos de dados e mensageria.
   **Métricas:** Monitoramento de saúde, latência, throughput e uso de recursos via APM (Application Performance Monitoring).

## Deploy

A plataforma segue o modelo **Cloud-Native**.
   Empacotamento de todos os serviços em *Containers*.
   Orquestração desenhada para alta disponibilidade, auto-scaling e resiliência (Self-healing).
   Infraestrutura como Código (IaC) e pipelines de CI/CD automatizados, garantindo que o código passe por análises de qualidade e segurança antes de chegar em produção.
