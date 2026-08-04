# Architecture Principles

Este documento responde a uma pergunta fundamental: **Como toda solução na Business Management Platform deve ser construída?**

As diretrizes abaixo não são sugestões; são regras arquiteturais inegociáveis. Elas garantem que, independentemente do desenvolvedor (humano ou Inteligência Artificial) ou do módulo sendo construído, o ecossistema mantenha um padrão unificado de excelência, previsibilidade e manutenibilidade.

## Domain Driven Design

O código deve ser um reflexo fiel do negócio.
   **Linguagem Ubíqua:** O código, os bancos de dados, as APIs e as conversas devem utilizar os mesmos termos do negócio. Rejeite jargões técnicos para nomear entidades de domínio.
   **Modelos Ricos:** As entidades de domínio devem encapsular dados e comportamentos. Rejeitamos modelos anêmicos (classes apenas com *getters* e *setters*). A validação de invariantes do negócio pertence ao modelo.
   **Bounded Contexts:** Respeite rigorosamente as fronteiras de cada contexto. Um módulo não deve acessar o banco de dados de outro módulo diretamente.

## Clean Architecture

O domínio é o centro do universo.
   **Regra de Dependência:** As dependências do código-fonte devem apontar apenas para dentro, em direção às regras de negócio (Domínio).
   **Independência de Frameworks:** O *Core* da aplicação não deve herdar classes ou implementar interfaces de bibliotecas externas de persistência (ex: Entity Framework, Hibernate) ou de infraestrutura web.
   **Adaptadores:** A comunicação com o mundo externo (Bancos, APIs de terceiros, UI) deve ser feita através de portas e adaptadores (*Ports and Adapters*).

## SOLID

A fundação da escrita de código orientado a objetos em nossa plataforma.
   **S (Single Responsibility):** Uma classe ou módulo deve ter um, e apenas um, motivo para mudar.
   **O (Open/Closed):** O software deve estar aberto para extensão, mas fechado para modificação. Use abstrações em vez de alterar código existente.
   **L (Liskov Substitution):** Classes derivadas devem ser substituíveis por suas classes bases sem quebrar o sistema.
   **I (Interface Segregation):** Muitas interfaces específicas de clientes são melhores do que uma interface de propósito geral.
   **D (Dependency Inversion):** Dependa de abstrações (interfaces), não de implementações concretas.

## CQRS

Separação clara entre comandos (escrita) e consultas (leitura).
   **Comandos (Commands):** Métodos que alteram o estado do sistema. Eles executam regras de negócio complexas, não retornam dados da entidade (retornam apenas o status de sucesso/falha via *Result Pattern*) e disparam eventos.
   **Consultas (Queries):** Métodos que apenas retornam dados e não alteram o estado do sistema. Podem ignorar o modelo de domínio complexo e consultar o banco de dados diretamente para máxima performance (ex: usando Dapper ou queries otimizadas).

## Event Driven

A comunicação inter-domínios deve minimizar o acoplamento temporal e lógico.
   **Assincronicidade por Padrão:** Sempre que uma ação em um módulo precisar desencadear ações em outros módulos (ex: Contrato Assinado -> Gerar Cobrança), utilize eventos de domínio publicados em um *Event Bus*.
   **Consistência Eventual:** Aceitamos que, em sistemas distribuídos, os dados podem não estar perfeitamente sincronizados em todos os nós no mesmo milissegundo.

## API First

O contrato dita o desenvolvimento, não o contrário.
   **Design-First:** As APIs (REST ou GraphQL) devem ser desenhadas e documentadas (ex: OpenAPI/Swagger) antes de qualquer linha de código de implementação ser escrita.
   **Versionamento:** Toda API deve ser versionada desde a versão 1. Mudanças que quebram contrato (*breaking changes*) exigem uma nova versão da API.
   **Agnosticismo de UI:** A API não deve ser construída pensando apenas em uma tela específica do Front-end, mas sim como uma capacidade de negócio consumível por qualquer cliente.

## Security First

A segurança é pensada no design, não na auditoria pré-lançamento.
   **Zero Trust:** Nunca confie na entrada de dados. Valide todas as requisições, mesmo as originadas internamente por outros microserviços.
   **Tenant Isolation Obrigatório:** Toda query, command ou evento deve carregar e validar o contexto do inquilino (`TenantId`). O vazamento de dados entre clientes é a falha mais crítica que pode ocorrer na plataforma.
   **Princípio do Menor Privilégio:** Serviços e usuários devem ter apenas as permissões estritamente necessárias para executar sua função (RBAC).

## Cloud Native

O software nasce pronto para a nuvem elástica.
   **Stateless:** As aplicações de API não devem guardar estado localmente em memória (sessões). Qualquer estado deve ser armazenado em bancos de dados ou caches distribuídos (ex: Redis).
   **Containerização:** Todo serviço deve ser empacotável em contêineres e ser orquestrável de forma transparente.
   **Graceful Degradation:** Se um serviço externo ou não-crítico cair, o sistema deve continuar operando com funcionalidades reduzidas, sem falhar completamente.

## Observabilidade

Se não podemos medir e rastrear, o código não está pronto para produção.
   **Logs Contextualizados:** Todo log deve conter estruturalmente o `CorrelationId`, `TenantId` e `UserId`.
   **Tracing Distribuído:** Todo fluxo de requisição que cruza mais de um serviço deve carregar um cabeçalho de trace rastreável de ponta a ponta.
   **Health Checks:** Todos os serviços devem expor endpoints para verificação de saúde (Liveness e Readiness) para a orquestração da nuvem.

## Escalabilidade

A plataforma deve crescer de forma horizontal e elástica.
   **Scale-Out:** Preferimos adicionar mais máquinas menores do que uma máquina maior (*Scale-Up*). O código não pode assumir que é a única instância em execução.
   **Background Processing:** Operações pesadas (geração de PDF, envio de lotes de e-mails) não devem segurar a requisição HTTP. Devem ser delegadas para *workers* assíncronos.

## Performance

Uso inteligente dos recursos de computação.
   **Caching Estratégico:** Dados de leitura frequente e alteração rara devem ser oxigenados em cache, reduzindo a carga nos bancos de dados transacionais.
   **N+1 Queries:** Evite rigorosamente problemas de *N+1* no acesso a banco de dados. Leituras em lote devem ser consolidadas.
   **Paginação:** Nenhuma API que retorna listas deve ser desenvolvida sem paginação obrigatória.

## Testabilidade

A confiança no *deploy* contínuo provém de uma malha de testes impenetrável.
   **Pirâmide de Testes:** Priorize testes de unidade (rápidos e em grande volume para a camada de domínio), seguidos por testes de integração (para infraestrutura/banco) e poucos testes *End-to-End* (E2E).
   **TDD/BDD:** Encoraja-se a escrita do teste antes da implementação, orientando o design a partir do comportamento esperado.
   **Isolamento:** Testes de unidade não devem tocar no disco rígido, em bancos de dados ou em serviços de rede externos. Use *Mocks* e *Stubs*.

## IA Assisted Development

O código é feito por humanos e máquinas, para humanos e máquinas.
   **Legibilidade para LLMs:** Escreva código e comentários claros, autoexplicativos e modulares. Arquivos gigantes com múltiplas responsabilidades confundem não apenas humanos, mas também perdem o contexto de agentes de IA (como o ClaudeCode).
   **Padrões Determinísticos:** O uso de templates padronizados e arquitetura rigorosa (*Framework Base*) permite que a IA gere de forma autônoma (e correta) entidades, repositórios e casos de uso inteiros sem "alucinar" na estrutura do projeto.
   **Code as Prompt:** Entendemos que nossos artefatos (interfaces, entidades, ADRs) servirão de contexto (*prompt*) contínuo para as inteligências artificiais que ajudam a evoluir a plataforma.
