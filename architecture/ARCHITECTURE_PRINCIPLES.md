# Nele registraremos regras como

O domínio nunca acessa infraestrutura diretamente.
Toda regra de negócio pertence ao domínio.
Comunicação entre módulos ocorre por contratos definidos.
APIs são versionadas.
Todos os casos de uso retornam um Result.
Toda entidade possui auditoria quando aplicável.
Multi-tenancy é tratado como requisito transversal.
Nenhum documento de detalhe pode contradizer um documento de nível superior.

Ou seja:

O PROJECT_CHARTER define a estratégia.
O BUSINESS_ARCHITECTURE define o produto.
O ARCHITECTURE_PRINCIPLES define as regras de construção.
O FRAMEWORK_GUIDE define como implementar essas regras.
Os domínios detalham a aplicação dessas regras.
