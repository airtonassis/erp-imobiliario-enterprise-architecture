---
description: Roda uma auditoria completa da arquitetura documental do ERP Imobiliário, domínio a domínio, usando o subagente architecture-validator
---

Rode uma auditoria completa da arquitetura documental deste repositório.

Escopo: $ARGUMENTS (se vazio, audite TODOS os domínios em `business/domain-model/*/`)

Passos:

1. Liste todos os domínios em `business/domain-model/` (cada subpasta é um domínio).
2. Para cada domínio, delegue ao subagente `architecture-validator` uma auditoria
   completa (completude estrutural, stubs, contaminação de template, IDs
   duplicados, nomenclatura, bounded context, rastreabilidade para módulos).
3. Consolide os resultados de todos os domínios em um único relatório:
   - Tabela geral: domínio | % completo | bloqueantes | importantes
   - Ranking dos domínios mais urgentes de corrigir antes de virar sprint de dev
   - Lista consolidada de achados 🔴 bloqueantes de todo o repositório (não só
     por domínio) — ex: IDs duplicados no catálogo global, que são um problema
     do repo inteiro, não de um domínio específico
4. Termine com uma recomendação objetiva: quais 2-3 domínios devem ser
   completados a seguir (considerando `roadmap/PRODUCT_BACKLOG.md` e a
   prioridade/sprint definida em `modules/MODULE_CATALOG.md`), e por quê.

Não corrija nada automaticamente nesta rodada — esta é uma auditoria somente
leitura. Se o usuário quiser corrigir os achados depois, isso é um passo
separado.
