# Property Entities

| Campo | Valor |
| Document ID | DOM-PROP-007|
| Nome | Property Entities |
| Domínio | Property |
| Categoria | Domain Model |
| Versão | 1.0.0 |
| Status | Draft |
| Autor | Airton Assis |
| Arquiteto | ChatGPT |
| Última Atualização | 31/07/2026 |

1. Introdução

2. Atores

3. Catálogo de Casos de Uso

4. Fluxos

5. Fluxos Alternativos

6. Fluxos de Exceção

7. Regras de Negócio Relacionadas

8. Eventos Gerados

9. APIs Relacionadas

10. Permissões

11. Critérios de Aceite

12. Rastreabilidade

Exemplo
UC-PROP-001
Cadastrar Imóvel

Objetivo

Cadastrar um imóvel para venda ou locação.

Atores

Administrador
Corretor
Gestor

Pré-condições

Tenant ativo.
Usuário autenticado.
Owner cadastrado.

Fluxo Principal

Informar dados básicos.
Informar endereço.
Informar características.
Informar proprietário.
Informar finalidade.
Salvar.
Gerar evento PropertyCreated.

Fluxos Alternativos

Salvar como rascunho.
Importar dados de integração.
Duplicar imóvel existente.

Exceções

CEP inválido.
Proprietário inexistente.
Tipo de imóvel inválido.
Código duplicado.

Regras Relacionadas

BR-PROP-001
BR-PROP-002
BR-PROP-010

Eventos

EVT-PROP-001

Permissões

PROPERTY_CREATE

Critérios de Aceite

Imóvel criado com sucesso.
Auditoria registrada.
Tenant associado corretamente.

Catálogo sugerido

O módulo Property provavelmente terá entre 25 e 40 casos de uso, por exemplo:

Cadastro
UC-PROP-001 – Cadastrar imóvel
UC-PROP-002 – Editar imóvel
UC-PROP-003 – Arquivar imóvel
UC-PROP-004 – Reativar imóvel
Conteúdo
UC-PROP-010 – Adicionar imagem
UC-PROP-011 – Remover imagem
UC-PROP-012 – Gerenciar documentos
UC-PROP-013 – Gerenciar vídeos
Comercial
UC-PROP-020 – Publicar imóvel
UC-PROP-021 – Suspender publicação
UC-PROP-022 – Alterar preço
UC-PROP-023 – Registrar proposta
UC-PROP-024 – Aprovar proposta
UC-PROP-025 – Registrar venda
UC-PROP-026 – Registrar locação
UC-PROP-027 – Reservar imóvel
Operacional
UC-PROP-030 – Agendar visita
UC-PROP-031 – Cancelar visita
UC-PROP-032 – Registrar avaliação
Marketing
UC-PROP-040 – Gerar anúncio
UC-PROP-041 – Publicar em portais
UC-PROP-042 – Destacar imóvel
