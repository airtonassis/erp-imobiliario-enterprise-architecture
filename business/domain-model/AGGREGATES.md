# Aggregate Root

1. Introdução

2. Aggregate Root
Property
│
├── PropertyAddress
├── PropertyLocation
├── PropertyOwner
├── PropertyImage
├── PropertyVideo
├── PropertyDocument
├── PropertyAttachment
├── PropertyFeature
├── PropertyAmenity
├── PropertyPrice
├── PropertyPriceHistory
├── PropertyPublication
├── PropertyPortalPublication
├── PropertyProposal
├── PropertyVisit
├── PropertyAvailability
├── PropertyReservation
├── PropertySEO
├── PropertyTag
├── PropertyHighlight
├── PropertyAudit
└── PropertyHistory

4.Limites do Aggregate

5.Entidades pertencentes

6.Invariantes

7.Operações permitidas
Property

↓

Adicionar imagem

↓

Atualizar endereço

↓

Alterar preço

↓

Adicionar proprietário

↓

Publicar imóvel

8.Operações proibidas
PropertyImage

↓

Atualizar diretamente

9.Regras transacionais

10.Eventos publicados

11.Diretrizes de implementação

## Invariantes

INV-001

Todo Property deve possuir um Owner.
INV-002

Todo Property publicado deve possuir Address válido.
INV-003

Todo Property deve possuir Status.
INV-004

Property vendido não pode voltar para Published sem autorização.
INV-005

Todo Property deve possuir Tenant.

## Operações do Aggregate

CreateProperty()

UpdateProperty()

Publish()

Archive()

Reserve()

Sell()

Rent()

AddImage()

RemoveImage()

AddDocument()

UpdatePrice()

AssignOwner()

RemoveOwner()

RegisterVisit()

CreateProposal()

## Eventos Publicados

PropertyCreated

PropertyUpdated

PropertyPublished

PropertyReserved

PropertySold

PropertyRented

PropertyArchived

PropertyPriceChanged

PropertyImageAdded

PropertyImageRemoved

PropertyOwnerAssigned

## Integração com outros modulos

| Módulo    | Tipo de relação |
| --------- | --------------- |
| Tenant    | Obrigatória     |
| Owner     | Obrigatória     |
| Customer  | Consulta        |
| Sales     | Consome eventos |
| Rentals   | Consome eventos |
| CRM       | Consulta        |
| Marketing | Consulta        |
| Portal    | Publicação      |
| Financial | Consome eventos |
