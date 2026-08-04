# Missão

Para estruturar a comunicação entre os módulos dentro da arquitetura que definimos (Clean Architecture + DDD + RabbitMQ + .NET 9), a regra de ouro é o **baixo acoplamento**. Os domínios não devem conversar diretamente com o banco de dados um do outro e, idealmente, não devem ter referências diretas em código uns aos outros.

Aqui está o desenho arquitetural de como cada uma dessas interações deve ocorrer na prática:

---

## 1. Como o módulo `Property` conversa com `Finance`?

Como a arquitetura conta com **RabbitMQ**, a melhor abordagem é orientada a eventos (*Event-Driven*), garantindo que `Property` (Imóveis) não dependa diretamente de `Finance` (Financeiro).

**O Fluxo:**

1. Uma ação ocorre em `Property` (ex: um imóvel tem sua taxa de administração alterada ou é alugado).
2. O módulo `Property` publica um evento de integração na mensageria (RabbitMQ). Exemplo: `PropertyRentedIntegrationEvent`.
3. O módulo `Property` continua sua vida e encerra a requisição do usuário (rápido e sem bloqueios).
4. O módulo `Finance`, que está "escutando" a fila do RabbitMQ (como um *Consumer* ou *BackgroundService*), recebe o evento.
5. `Finance` processa as regras dele: gera as contas a receber, faturas de comissão e repasses do proprietário.

**Vantagem:** Se o módulo `Finance` cair ou estiver em manutenção, o aluguel do imóvel não é bloqueado. O RabbitMQ guarda a mensagem até que o `Finance` volte e a processe.

---

### 2. Como `CRM` aciona `Vendas`?

Neste caso, o CRM lida com *Leads* e Funil. Quando um negócio é fechado (Ganho), ele precisa virar uma Venda real. Aqui temos duas opções recomendadas:

**Opção A (Eventos Assíncronos via RabbitMQ - Recomendado):**

* O corretor arrasta o card no CRM para "Fechado".
* O CRM altera o status do Lead e dispara o evento `DealWonIntegrationEvent`.
* O módulo de `Vendas` (ou `Contracts`) consome esse evento e cria automaticamente um "Rascunho de Venda", puxando os dados do Lead (Cliente, Imóvel, Valor).

**Opção B (Comunicação Síncrona Interna via Interfaces/MediatR):**

* Se for necessário que a tela de CRM já retorne o número do contrato/venda imediatamente, o `Application` do CRM pode chamar uma Interface do módulo de Vendas (ex: `ISalesService.CreateDraftAsync(LeadDto)`).
* *Atenção:* O CRM conversa com a camada de `Application` de Vendas, **nunca** com a `Infrastructure` ou banco de dados de Vendas.

---

### 3. Como `Contratos` gera eventos?

Seguindo o padrão de **Domain Events** (Eventos de Domínio) que mapeamos no seu `ERP.Framework`, a geração acontece de dentro para fora, garantindo que o banco só atualize se o evento for válido.

**O Fluxo:**

1. **Domínio:** Dentro da Entidade `Contract`, ao chamar o método `Sign()` (Assinar), a própria entidade registra o evento na memória.
2. **Application / Unit of Work:** O manipulador (*Command Handler*) manda salvar no banco (`SaveChanges`).
3. **Framework:** O `Unit of Work` intercepta o `SaveChanges`. Antes ou logo após o *commit* no PostgreSQL, ele pega todos os `DomainEvents` acumulados na entidade e os publica (usando o `MediatR`, por exemplo).
4. **Disparo Externo:** Um *Handler* escuta esse `DomainEvent` e, se necessário, o transforma em um `IntegrationEvent` enviando para o RabbitMQ para que outros módulos (como Financeiro) saibam da assinatura.

---

### 4. Como Notificações são disparadas?

O módulo de Notificações deve ser um **módulo genérico e reativo**. Ele não possui regras de negócios de imóveis ou finanças; ele apenas sabe enviar e-mails, SMS ou Push.

**O Fluxo:**

1. O módulo de Notificações apenas escuta diversas filas do RabbitMQ (ex: `ContractSigned`, `BoletoOverdue`, `ViewingScheduled`).
2. Qualquer outro módulo da aplicação que precise notificar o usuário apenas "joga" um evento genérico na fila, como `SendEmailIntegrationEvent` (contendo destinatário, template e variáveis).
3. O *Worker* de Notificações consome a mensagem e dispara pela API do serviço de e-mail ou SMS.

---

### 5. Quais dependências são permitidas? (Regras de Arquitetura)

Para manter o *Clean Architecture* e o projeto modular sem virar um "código espaguete", estas devem ser as regras de dependência (validadas por ferramentas como *ArchUnitNET* na sua suíte de testes):

**Dependências de Camadas (Dentro de um módulo, ex: Property):**

* **Domain:** Não depende de NENHUMA outra camada. É o coração.
* **Application:** Depende apenas do **Domain**.
* **Infrastructure:** Depende do **Application** (para implementar as interfaces, como Repositórios) e do **Domain**.
* **API / Presentation:** Depende da **Application** e da **Infrastructure** (somente para Injeção de Dependência na inicialização).

**Dependências entre Módulos (Property vs Finance vs CRM):**

* Um módulo **NÃO PODE** acessar o DbContext do outro módulo.
* Um módulo **NÃO PODE** referenciar diretamente o projeto `Infrastructure` do outro módulo.
* A comunicação entre eles deve ocorrer através de:
* **Eventos de Integração (RabbitMQ)** (Preferencial).
* **Camada de Shared Kernel/Framework** (Interfaces compartilhadas e DTOs comuns).
* **Chamadas de API internas** (caso o ERP no futuro seja separado em Microserviços reais).
  