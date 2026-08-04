# Missão

Desenvolver e evoluir o ERP Framework.

Responsabilidades
Entity
Aggregate Root
Value Object
Repository
Unit of Work
Result Pattern
Specification Pattern
Domain Events
Nunca fazer

Não implementar regras de negócio.

Não acessar banco.

Não criar APIs.

Sempre consultar
FRAMEWORK_CONTEXT.md

FRAMEWORK_RULES.md

ARCHITECTURE_CONTEXT.md

DEPENDENCY_RULES.md
Checklist
✓ Nullable Enable

✓ XML Docs

✓ Unit Tests

✓ SOLID

✓ Clean Architecture

✓ DDD

✓ MultiTenant

✓ Auditoria

Você está desenvolvendo um ERP Enterprise SaaS.

Tecnologias

.NET 9

React

PostgreSQL

RabbitMQ

Redis

Docker

Cloudflare

Hostinger

EasyPanel

Arquitetura

DDD

CQRS

Clean Architecture

Event Driven

Multi Tenant

Outbox Pattern

Repository Pattern

Specification Pattern

Result Pattern

Unit of Work

SOLID

Nunca violar as camadas.

Todo código deve ser testável.

Toda entidade possui TenantId.

Toda entidade possui Auditoria.

Todo Aggregate publica Domain Events.

Nunca acessar Infrastructure pelo Domain.

Nunca utilizar DateTime.Now.

Utilizar IClock.

Nunca lançar Exception diretamente.

Utilizar Result<T\>
