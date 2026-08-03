# ERP Imobiliário Enterprise — Instruções para Claude Code

## O que é este repositório

Este NÃO é (ainda) um repositório de código. É a **arquitetura documental** de um
ERP/CRM imobiliário SaaS, seguindo DDD + Clean Architecture + Event Driven +
Modular Monolith. O código será gerado a partir desta documentação, então a
qualidade e consistência dos documentos aqui é o que determina a qualidade do
sistema depois.

Stack alvo (ver `.ai/MASTER_CONTEXT.md` e `architecture/TECH_STACK.md`):
Java 21 + Spring Boot 3 (backend), React/Next.js/TypeScript (frontend),
Flutter (mobile), PostgreSQL + Redis + RabbitMQ + MinIO, Docker/EasyPanel/Cloudflare/Hostinger.

## Estado atual (não assuma que está completo)

- Apenas o domínio **`business/domain-model/property/`** está de fato desenvolvido
  (README, ENTITIES, VALUE_OBJECTS, AGGREGATES, BUSINESS_RULES, DOMAIN_EVENTS,
  USE_CASES, DATABASE_MODEL, API_SPECIFICATION, PERMISSIONS, VALIDATIONS,
  TEST_SCENARIOS, WORKFLOWS, ENTITY_RELATIONSHIP_MATRIX). **Use este domínio
  como golden standard / referência de qualidade** ao avaliar ou gerar os demais.
- Todos os outros domínios (customer, contract, sale, rental, financial,
  commission, communication, marketing, owner, payment, proposal, inspection,
  visit, generic, supporting, core, Tenant) têm apenas arquivos-stub de ~289
  bytes com metadata de template, sem conteúdo real. Vários ainda têm o texto
  copiado literalmente do template Property sem substituição (ex.:
  `customer/README.md` tem o título "Property Entities").
- `docs/DEVELOPMENT_READINESS.md` reflete o status apenas do domínio Property:
  Domain Model / Business Rules / Use Cases = feitos; Database / APIs /
  Security / Permissions / Test Scenarios / OpenAPI / Deployment = pendentes.
- `docs/DOCUMENT_CATALOG.md` tem IDs duplicados e inconsistências de nomenclatura
  (ex.: "Bussiness_RULLES", "VALIDACIONS") — provavelmente geradas por IA sem
  revisão humana. Trate como não confiável até validado.
- Pasta `Tenant/` está com maiúscula, quebrando o padrão lowercase-com-hífen
  usado no resto do projeto.

## Regras gerais do projeto (de `.ai/MASTER_CONTEXT.md`)

- Nunca alterar módulos fora do escopo da tarefa.
- Sempre gerar testes.
- Sempre documentar APIs (Swagger/OpenAPI).
- Sempre usar DTO e Mapper entre camadas.
- Sempre usar Flyway para migrations.

## Como trabalhar neste repositório

1. **Antes de gerar qualquer artefato novo** (entidade, use case, endpoint),
   confira se o domínio já tem um README com bounded context, responsabilidades
   e "fora do escopo" definidos. Se não tiver, isso é bloqueante — sinalize em
   vez de inventar.
2. **Ao preencher um domínio stub**, siga a estrutura e nível de detalhe do
   domínio Property (README → ENTITIES → VALUE_OBJECTS → AGGREGATES →
   BUSINESS_RULES → DOMAIN_EVENTS → USE_CASES → DATABASE_MODEL →
   API_SPECIFICATION → PERMISSIONS → VALIDATIONS → TEST_SCENARIOS → WORKFLOWS).
   Use os templates em `templates/documents/` como esqueleto formal, mas o
   conteúdo real deve ter a profundidade do domínio Property.
3. **Nunca duplique um Document ID** já usado em `docs/DOCUMENT_CATALOG.md`.
   Ao criar um documento novo, adicione a linha correspondente no catálogo.
4. **Terminologia em português para o domínio de negócio** (nomes de entidades,
   regras, personas), mas nomes técnicos (classes, campos, endpoints) em inglês,
   seguindo `governance/engineering/NAMING_CONVENTIONS.md`.
5. Use o subagente `architecture-validator` (`.claude/agents/architecture-validator.md`)
   ou o comando `/validate-architecture` para checar consistência antes de considerar
   um domínio "pronto para desenvolvimento".

## Onde procurar contexto

- Visão de negócio: `business/domain-model/**`, `business/personas/`, `business/journeys/`
- Decisões de arquitetura: `adr/`, `.ai/context/DECISIONS.md`
- Padrões de referência (DDD, Clean Arch, CQRS, Hexagonal, Segurança, DB): `architecture/reference-architecture/`
- Convenções de código/API/DB/log/erro: `standards/`
- Checklists de qualidade: `governance/quality/`
- Roadmap e backlog: `roadmap/PRODUCT_BACKLOG.md`, `roadmap/RELEASE_PLAN.md`
