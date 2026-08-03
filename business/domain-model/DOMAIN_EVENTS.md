# Eventos

PropertyCreated

PropertyPublished

PropertySold

PropertyRented

PropertyUpdated

PropertyArchived

PropertyPriceChanged

PropertyOwnerChanged

## Estrutura Sugerida

1. Introdução

2. Conceitos

3. Lista de Eventos

4. Publicadores

5. Consumidores

6. Payload

7. Versionamento

8. Retry

9. Idempotência

10. Segurança

11. Observabilidade

12. Integrações

13. Fluxos

14. Evolução futura

## Catalago de Eventos

## Cadastro

EVT-PROP-001

PropertyCreated

EVT-PROP-002

PropertyUpdated

EVT-PROP-003

PropertyDeleted

EVT-PROP-004

PropertyArchived

## Publicação

EVT-PROP-010

PropertyPublished

EVT-PROP-011

PropertyUnpublished

## Comercial

EVT-PROP-020

PropertyReserved

EVT-PROP-021

ReservationExpired

EVT-PROP-022

ProposalCreated

EVT-PROP-023

ProposalAccepted

EVT-PROP-024

PropertySold

EVT-PROP-025

PropertyRented

## Conteudo

EVT-PROP-030

ImageAdded

ImageRemoved

DocumentAdded

VideoAdded

## Financeiro

EVT-PROP-040

PriceChanged

CommissionChanged

## Auditoria

EVT-PROP-050

AuditGenerated

## Para Cada Evento

## EVT-PROP-024

## Nome

PropertySold

---

## Descrição

Indica que o imóvel foi vendido.

---

## Publicador

Property Aggregate

---

## Consumidores

Sales

Financial

CRM

Notification

Analytics

Portal

---

## Payload

PropertyId

TenantId

OwnerId

CustomerId

BrokerId

SaleId

Price

Date

User

CorrelationId

---

## Garantia

At Least Once

---

## Idempotência

Obrigatória

---

## Versionamento

v1

---

## Auditorias

Sim

---

## Segurança

Tenant obrigatório.

## Fluxo

PropertySold

↓

RabbitMQ

↓

Financial

↓

CRM

↓

Notification

↓

Portal

↓

Analytics
