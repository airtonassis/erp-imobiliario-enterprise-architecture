# Property Domain

| Document ID | DOM-PROP-001 |
| Nome | Property Domain Overview |
| Domínio | Property |
| Categoria | Domain Model |
| Versão | 1.0.0 |
| Status | Draft |
| Autor | Airton Assis |
| Arquiteto | ChatGPT |
| Última Atualização | 31/07/2026 |

> Domain Owner: Real Estate Business Team
>
> Bounded Context: Property Management
>
> Module: Property
>
> Version: 1.0.0

---

## Objetivo

O domínio Property é responsável por todo o ciclo de vida de um imóvel dentro do ERP Imobiliário Enterprise.

Este domínio gerencia imóveis disponíveis para venda, locação, temporada, empreendimentos, terrenos, imóveis rurais e imóveis comerciais.

Todo imóvel existente no sistema pertence obrigatoriamente a este domínio.

O domínio Property representa o núcleo operacional da imobiliária e serve como base para diversos outros módulos, como CRM, Vendas, Locação, Financeiro, Marketing e Integrações.

---

## Responsabilidades

O domínio Property é responsável por:

- Cadastro de imóveis
- Atualização de imóveis
- Publicação
- Despublicação
- Arquivamento
- Gestão de fotos
- Gestão de vídeos
- Gestão de documentos
- Gestão de proprietários
- Gestão de localização
- Gestão de características
- Gestão de valores
- Histórico de alterações
- Histórico de preços
- Histórico de proprietários
- Publicação em portais
- Controle de disponibilidade
- Controle de status
- Indexação para pesquisa
- SEO
- Integração com IA

---

## Fora do Escopo

Este domínio NÃO possui responsabilidade sobre:

- Clientes
- Corretores
- Contratos
- Financeiro
- Pagamentos
- CRM
- Marketing
- Agenda
- Workflow Comercial

Esses domínios apenas utilizam informações do Property.

---

## Objetivos do Domínio

O objetivo principal deste domínio é manter a representação única, consistente e íntegra de todos os imóveis da organização.

Todo imóvel deverá possuir um identificador único e um ciclo de vida totalmente rastreável.

---

## Tipos de Imóveis

O domínio suporta:

- Casa
- Apartamento
- Cobertura
- Kitnet
- Studio
- Loft
- Sala Comercial
- Galpão
- Terreno
- Chácara
- Fazenda
- Sítio
- Prédio Comercial
- Empreendimento
- Imóvel Industrial

---

## Finalidades

Um imóvel pode possuir uma ou mais finalidades.

Exemplos:

- Venda
- Locação
- Temporada
- Permuta
- Lançamento

---

## Status do Imóvel

Exemplos:

- Draft
- Em Análise
- Disponível
- Reservado
- Proposta
- Vendido
- Alugado
- Suspenso
- Arquivado

---

## Dependências

Este domínio depende de:

Owner

Address

Document

Media

Authentication

Storage

Notification

Workflow

Audit

Search

---

## Consumidores

Este domínio é utilizado por:

CRM

Sales

Rentals

Contracts

Marketing

Portal

Dashboard

AI

Analytics

Integrations

---

## Eventos Publicados

PropertyCreated

PropertyUpdated

PropertyDeleted

PropertyPublished

PropertyArchived

PropertySold

PropertyRented

PropertyPriceChanged

PropertyOwnerChanged

PropertyImagesUpdated

PropertyLocationChanged

---

## Eventos Consumidos

OwnerUpdated

ContractClosed

PaymentConfirmed

CustomerInterested

LeadCreated

WorkflowApproved

---

## Principais Entidades

Property

PropertyAddress

PropertyImage

PropertyVideo

PropertyDocument

PropertyOwner

PropertyFeature

PropertyPriceHistory

PropertyPublication

PropertyVisit

PropertyLead

PropertySEO

---

## Aggregate Root

Aggregate Root:

Property

Todo acesso ao domínio deverá ocorrer através do Aggregate Property.

Nenhuma entidade filha poderá ser manipulada diretamente.

---

## Integrações

Google Maps

Google Geocoding

ViaCEP

Cloudflare Images

AWS S3

MinIO

Zap Imóveis

Viva Real

OLX

Meta

Instagram

Facebook

WhatsApp Business

OpenAI

Claude

RabbitMQ

Redis

Elasticsearch

---

## Regras Gerais

Todo imóvel deve possuir proprietário.

Todo imóvel deve possuir endereço válido.

Todo imóvel deve possuir categoria.

Todo imóvel deve possuir tipo.

Todo imóvel deve possuir status.

Todo imóvel publicado deve possuir imagens.

Todo imóvel vendido não poderá voltar para disponível sem autorização.

Todo imóvel alugado deverá possuir contrato ativo.

Todo imóvel arquivado ficará somente para consulta.

---

## Segurança

Permissões:

Visualizar

Cadastrar

Editar

Excluir

Publicar

Arquivar

Alterar Proprietário

Alterar Valores

Exportar

Importar

---

## Auditoria

Todas as alterações deverão ser auditadas.

Registrar:

Usuário

Data

Hora

IP

Origem

Valores antigos

Valores novos

---

## KPIs

Quantidade de imóveis

Imóveis publicados

Imóveis vendidos

Imóveis alugados

Tempo médio de venda

Tempo médio de locação

Preço médio

Ticket médio

Visitas

Conversões

---

## Próximos Documentos

ENTITIES.md

VALUE_OBJECTS.md

AGGREGATES.md

BUSINESS_RULES.md

DOMAIN_EVENTS.md

USE_CASES.md

DATABASE_MODEL.md

API_SPECIFICATION.md

VALIDATIONS.md

PERMISSIONS.md

TEST_SCENARIOS.md
