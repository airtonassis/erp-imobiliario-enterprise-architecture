# Property Entities

| Campo | Valor |
| Document ID | DOM-PROP-002 |
| Nome | Property Entities |
| Domínio | Property |
| Categoria | Domain Model |
| Versão | 1.0.0 |
| Status | Draft |
| Autor | Airton Assis |
| Arquiteto | ChatGPT |
| Última Atualização | 31/07/2026 |

## Objetivo

Representa um imóvel cadastrado no ERP.

---

## Responsabilidades

- Controlar ciclo de vida
- Controlar disponibilidade
- Centralizar informações do imóvel
- Publicação
- Integrações

---

## Aggregate Root

Sim

---

## Entidades Filhas

PropertyAddress

PropertyImage

PropertyVideo

PropertyDocument

PropertyOwner

PropertyFeature

PropertyPublication

PropertyPriceHistory

PropertyVisit

---

## Consumida por

Sales

Rentals

CRM

Marketing

Contracts

Analytics

---

## Eventos Publicados

PropertyCreated

PropertyUpdated

PropertyPublished

PropertyArchived

PropertySold

PropertyRented

---

## Observações

Nenhuma entidade poderá alterar Property sem passar pelo Aggregate Root.
