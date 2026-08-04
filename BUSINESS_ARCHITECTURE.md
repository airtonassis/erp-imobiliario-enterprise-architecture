# Business Architecture

Este documento mapeia a estrutura conceitual do negócio. Ele traduz a operação corporativa e as estratégias de mercado em domínios, fluxos e regras claras, garantindo que o software a ser construído seja um reflexo fiel da realidade da empresa.

## Domínios

Os domínios representam as grandes áreas de conhecimento e atuação da empresa. Eles definem "o que" o negócio faz. Na nossa plataforma, os domínios são classificados por sua importância estratégica:
   **Core Domain (Domínio Principal):** É o coração do negócio atual, onde reside o diferencial competitivo. Para o nosso primeiro produto, é o **Real Estate** (Gestão Imobiliária de Ponta a Ponta).
   **Generic Domains (Domínios Genéricos):** Áreas essenciais para qualquer empresa, mas que não são um diferencial de mercado por si só. Inclui **Finanças**, **CRM** (Gestão de Relacionamento) e **Gestão de Documentos**.
   **Supporting Domains (Domínios de Suporte):** Áreas que apoiam o ecossistema, como **Comunicação/Notificações** e **Auditoria**.

## Subdomínios

Os domínios são divididos em subdomínios, que agrupam capacidades corporativas mais específicas:
   **Subdomínios de Real Estate:** Captação de Imóveis, Gestão de Contratos de Locação, Vendas, Vistorias e Manutenções.
   **Subdomínios Financeiros:** Contas a Pagar, Contas a Receber, Faturamento, Split de Pagamentos (Repasses e Comissões).
   **Subdomínios de CRM:** Gestão de Leads, Qualificação, Atendimento ao Cliente (Pós-Venda).

## Bounded Contexts

Os Contextos Delimitados (*Bounded Contexts*) definem as fronteiras linguísticas do negócio. Um mesmo termo pode ter significados diferentes dependendo do contexto (Linguagem Ubíqua):
   **Contexto de Catálogo (Imóveis):** O foco são as características físicas do ativo (metragem, endereço, comodidades).
   **Contexto de Locação:** O ativo passa a ser o objeto de um "Contrato", onde as partes envolvidas são "Locador", "Locatário" e "Fiador".
   **Contexto Financeiro:** As partes não importam pelo seu papel no imóvel, mas sim como "Pagador" ou "Recebedor". O "Contrato" se torna uma "Fatura" ou "Repasse".
   **Contexto de CRM:** Qualquer entidade é tratada inicialmente como um "Lead" ou "Contato", focando no relacionamento, histórico de interações e preferências.

## Capacidades de Negócio

As capacidades descrevem as habilidades e competências que a organização possui para gerar valor, independentemente de como os processos são executados:
   **Gestão de Ativos Imobiliários:** Capacidade de catalogar, precificar e disponibilizar imóveis para o mercado.
   **Análise de Risco e Crédito:** Capacidade de avaliar a saúde financeira de potenciais locatários e compradores.
   **Orquestração de Contratos:** Capacidade de gerar, negociar, aprovar e assinar documentos com validade jurídica.
   **Gestão de Ciclo Financeiro Completo:** Capacidade de cobrar, receber, reter taxas (administração/impostos) e repassar valores aos beneficiários finais.
   **Atendimento Autônomo e Contextual:** Capacidade de utilizar Inteligência Artificial para resolver dúvidas, renegociar dívidas e agendar visitas sem intervenção humana.

## Fluxos

As grandes cadeias de valor (Value Streams) que cruzam diversos departamentos e contextos para entregar um resultado final:
   **Lead to Contract (Da Captação ao Contrato):** O fluxo que começa com a entrada de um cliente interessado, passa por agendamentos de visita, proposta, análise de crédito, elaboração de documentos e culmina na assinatura do contrato.
   **Contract to Cash (Do Contrato ao Dinheiro):** O ciclo de vida do contrato ativo, gerando faturamentos mensais, cobranças ativas, recebimento, desconto de taxas da imobiliária e repasse líquido ao proprietário.
   **Issue to Resolution (Do Problema à Solução):** O fluxo de pós-venda, englobando a abertura de um chamado (ex: vazamento no imóvel), acionamento de prestadores de serviço, aprovação de orçamentos e resolução do reparo.

## Jornadas

As experiências projetadas sob a ótica dos diferentes atores que interagem com o negócio:
   **Jornada do Proprietário:** Focada em transparência e rentabilidade. Ele deseja ver seus imóveis alugados rapidamente, receber seus repasses em dia e ter acesso fácil aos extratos financeiros e laudos de vistoria.
   **Jornada do Locatário/Comprador:** Focada em agilidade e autoatendimento. Ele quer buscar imóveis, enviar documentação sem burocracia, assinar digitalmente, emitir segundas vias de boletos e solicitar reparos pelo celular.
   **Jornada do Corretor:** Focada em conversão. Ele precisa gerenciar sua agenda de visitas, ter histórico das conversas com os leads e acompanhar suas comissões projetadas.

## Processos

Passos operacionais que compõem os fluxos do negócio. Exemplo prático do **Processo de Locação Padrão**:
 Recebimento da Proposta Comercial.
 Reserva temporária do Imóvel (bloqueio no catálogo).
 Coleta de Documentos (Locatário e Garantidor).
 Aprovação de Crédito e Validação de Garantia.
 Geração e Assinatura Eletrônica do Contrato e Laudo de Vistoria de Entrada.
 Liberação das Chaves.
 Geração da primeira régua de faturamento (Aluguel pro-rata).

## Regras

Invariantes corporativas. Regras inegociáveis que protegem a integridade da operação comercial:
   Um imóvel não pode ser anunciado se o seu status for "Inativo" ou se já possuir um contrato de locação "Vigente".
   Um contrato de locação não pode entrar em vigor sem a assinatura eletrônica de todas as partes envolvidas e aprovação formal da modalidade de garantia.
   Nenhum repasse financeiro a um proprietário pode ser liquidado antes da efetiva compensação do pagamento pelo locatário.
   Taxas de administração da plataforma (Receita da Imobiliária) devem ser retidas automaticamente na fonte durante o processo de *Split* de pagamento.

## Eventos

Acontecimentos significativos no domínio de negócio que disparam reações em outros departamentos da empresa:
   `LeadQualificado`: Dispara a alocação de um corretor e agendamento de visita.
   `ContratoAssinado`: Dispara o arquivamento de documentos, geração das faturas financeiras e envio do manual do morador.
   `PagamentoAtrasado`: Dispara o fluxo da Régua de Cobrança (envio de lembretes via IA e cálculo de multas).
   `VistoriaReprovada`: Dispara o bloqueio da devolução do caução e abertura de ordem de serviço para manutenção.

## Integrações

Pontos de contato estratégico do negócio com parceiros, fornecedores e o mundo exterior:
   **Bureaus de Crédito e Background Check:** Para análise de risco, pontuação de crédito (Score) e verificação de antecedentes.
   **Portais Imobiliários:** Canais de vitrine (Marketplaces) para onde o catálogo de imóveis é exportado e de onde os leads são originados.
   **Instituições Financeiras:** Parceiros bancários e adquirentes para emissão de cobranças, processamento de pagamentos e liquidação de repasses.
   **Seguradoras e Garantidoras:** Entidades externas responsáveis por emitir apólices de seguro incêndio, seguro fiança e títulos de capitalização atrelados aos contratos.
   **Entidades Governamentais:** Prefeituras e secretarias da fazenda para consulta de IPTU e emissão de Notas Fiscais de Serviços.
