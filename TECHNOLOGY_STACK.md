# Enterprise Technology Standards

Este documento detalha o ecossistema tecnológico padronizado para a construção e operação da **Business Management Platform**. O uso estrito desta stack garante consistência, segurança e manutenibilidade entre as diferentes equipes, produtos e agentes de IA que interagem com o código.

## Backend Strategy

O núcleo de processamento e as lógicas de domínio da plataforma.
   **Linguagem:** C# (.NET 8+) ou TypeScript (Node.js) *(Ajuste conforme sua preferência)*.
   **Frameworks Base:** ASP.NET Core (para APIs REST e gRPC) ou NestJS.
   **Padrões de Acesso a Dados:** Entity Framework Core (para operações complexas de domínio/Commands) e Dapper (para leitura otimizada/Queries).
   **Comunicação Interna:** gRPC para comunicação síncrona de baixa latência entre microserviços internos.

## Frontend Engineering Strategy

A interface das aplicações web voltadas para o usuário final e painéis administrativos.
   **Ecossistema:** Next.js (React) utilizando TypeScript.
   **Estilização:** Tailwind CSS acoplado a um Design System interno (Component Library) baseada em Radix UI ou Shadcn/ui.
   **Gerenciamento de Estado:** Zustand (para estado global leve) e React Query / SWR (para cache e sincronização de dados de API).
   **Arquitetura UI:** Micro-frontends (Module Federation) para permitir que diferentes produtos (Core, Real Estate, Finance) sejam desenvolvidos e implantados de forma independente.

## Mobile

Aplicações nativas para parceiros, corretores e clientes finais.
   **Framework:** React Native (utilizando Expo) ou Flutter.
   **Justificativa:** Compartilhamento de lógica de estado com a web e velocidade de desenvolvimento multiplataforma (iOS e Android) com uma única base de código.

## Banco de Dados

 Banco relacional padrão
  Tecnologia homologada:
** PostgreSQL

Alternativas futuras:
**Azure SQL
**Amazon Aurora PostgreSQL
Critérios:
• ACID
• Escalabilidade
• Custos
• Suporte Cloud
• Multi Tenant

## Cache

Estratégias para mitigar latência e reduzir a carga transacional.
   **Cache Distribuído:** Redis. Utilizado para cache de consultas (Queries CQRS), gerenciamento de sessões distribuídas, rate limiting e armazenamento de metadados temporários.

## Mensageria

Tecnologia padrão:
**RabbitMQ:
** Alternativas homologadas:
**Apache Kafka
** Azure Service Bus
** AWS SQS/SNS

Critérios de escolha:
• Volume
• Throughput
• Garantia de entrega
• Complexidade operacional

## APIs

Contratos e exposição de serviços para o mundo exterior e Frontends.
   **Padrão Externo:** RESTful APIs maduras (Nível 3 de Richardson) para parceiros e integrações.
   **Padrão Frontend:** GraphQL (opcional via Apollo Server/BFF) para permitir que os clientes mobile e web solicitem exatamente os dados que precisam, reduzindo o tráfego de rede (Over-fetching).
   **Documentação:** Swagger (OpenAPI 3.0) gerado automaticamente a partir do código.

## IA

A base do ecossistema AI-First, garantindo autonomia e contextualização.
   **Modelos Base (LLMs):** Anthropic Claude 3.5 Sonnet / OpenAI GPT-4o (via chamadas de API).
   **Orquestração e Agentes:** LangChain ou Semantic Kernel (para encadeamento de prompts, criação de agentes autônomos e execução de *Tools* nas APIs do Core).
   **Assistência no Desenvolvimento:** ClaudeCode integrado ao fluxo de engenharia, atuando sobre os repositórios mapeados.

## DevOps

   **Infraestrutura como Código (IaC):** Terraform ou Pulumi. Nenhuma infraestrutura é provisionada manualmente via console.
   **Gestão de Configuração:** Ansible ou AWS Systems Manager.

## Cloud Strategy

   **Provedor Padrão:** AWS (Amazon Web Services) ou Microsoft Azure.
   **Serviços Chave:** Managed Kubernetes (EKS/AKS), Relational Database Service (RDS), S3/Blob Storage (para armazenamento de documentos, imagens e contratos).

## Containers

   **Empacotamento:** Docker. Toda aplicação e serviço deve conter um `Dockerfile` otimizado e seguro.
   **Orquestração:** Kubernetes (K8s). Responsável pelo *auto-scaling*, *self-healing* e roteamento interno via *Service Mesh* (ex: Istio).

## CI/CD

Integração e Entrega Contínuas garantindo a qualidade desde o commit até a produção.
   **Pipelines:** GitHub Actions.
   **Fluxo de Integração:** Linting rigoroso, execução de Testes Unitários e de Integração, análise de cobertura (SonarQube) e build de imagem Docker em todo *Pull Request*.
   **Fluxo de Deploy (GitOps):** ArgoCD orquestrando a sincronização contínua das imagens geradas diretamente para os clusters Kubernetes.

## Observabilidade

Sem visibilidade, não há disponibilidade.
   **Instrumentação:** OpenTelemetry (Padronização neutra de métricas, logs e traces).
   **Métricas e Dashboards:** Prometheus e Grafana.
   **Agregação de Logs:** ELK Stack (Elasticsearch, Logstash, Kibana) ou Datadog.
   **Application Performance Monitoring (APM):** Jaeger (para *Distributed Tracing*) ou ferramentas de mercado como New Relic/Datadog.

## Segurança

Adoção estrita de padrões de nível Enterprise e modelo Zero Trust.
   **Identity Provider (IdP):** Keycloak (Self-hosted) ou AWS Cognito / Auth0 para gestão de identidades, tokens JWT e fluxos OAuth2 / OpenID Connect.
   **Gestão de Segredos:** HashiCorp Vault. Nenhuma chave de API ou senha de banco de dados reside no código-fonte ou em variáveis de ambiente não criptografadas.
   **Proteção de Borda:** Cloudflare ou AWS WAF (Web Application Firewall) protegendo o API Gateway.

## Ferramentas

   **Versionamento:** Git / GitHub.
   **Design e Prototipagem:** Figma.
   **Documentação e Colaboração:** Markdown nos repositórios (Docs-as-code) e Notion/Confluence para atas gerenciais.
   **Comunicação:** Slack / Microsoft Teams integrados aos alertas de CI/CD e Observabilidade.

## Padrões

Adoção de padrões abertos da indústria para garantir interoperabilidade.
   **Autenticação:** OAuth 2.0 e OIDC.
   **APIs:** OpenAPI Specification 3.0 e AsyncAPI (para documentação do Event Bus).
   **Arquitetura:** C4 Model (Context, Containers, Components, Code) para diagramação de arquitetura.
   **Mensageria:** CloudEvents specification.

## Technology Decision Principles

Toda tecnologia incorporada à plataforma deve:
Ser Open Source ou possuir comunidade madura
Ter suporte Enterprise
Possuir documentação oficial
Ser compatível com containers
Permitir automação
Ser observável
Ser compatível com Kubernetes
Ser compatível com GitOps
Possuir roadmap ativo

## Version Strategy

.NET
*Sempre utilizar LTS
Node
*Sempre utilizar LTS
React
*Última versão estável
PostgreSQL
*N-1
Redis
*Última versão estável homologada
RabbitMQ
*Versão Enterprise homologada

## Ia Stank

AI Stack
Model Providers
Prompt Engine
RAG
Embeddings
Knowledge Base
Memory
Agents
Tool Calling
MCP
Evaluation
Observability
Guardrails
Safety

## Development Tools

VSCode
JetBrains Rider
Docker Desktop
GitHub Desktop
Claude Code
GitHub Copilot
Postman
Bruno
DBeaver
Lens
k9s
