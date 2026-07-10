# MILESTONE-08-Notifications

## Objetivo

Implementar a arquitetura de notificações básicas do Orbit.

## Dependências

* MILESTONE-01-Foundation
* MILESTONE-02-Authentication
* MILESTONE-04-Transactions
* MILESTONE-06-Goals

## Épico 1 — Eventos de notificação

### Descrição

Mapear os eventos do domínio que geram notificações.

### Objetivo

Cobrir alertas relevantes para contas, metas e orçamento.

### Dependências

Transações e metas.

### Feature 8.1 — Tipos de alerta

**Objetivo:** implementar o escopo das notificações do MVP.  
**Descrição:** organizar alertas por regra de negócio.  
**Critérios de aceitação:** tipos de alerta documentados.

#### Tasks

* **ID:** F08.01.01
  * **Título:** Implementar eventos geradores de notificação
  * **Descrição:** criar quais condições geram notificações.
  * **Prioridade:** Média
  * **Dependências:** F06.01.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** lista de eventos fechada.
  * **Definition of Done:** notificações básicas especificadas.

## Épico 2 — Persistência e leitura

### Descrição

Persistir notificações e permitir visualização no frontend.

### Objetivo

Garantir histórico e estado de leitura.

### Dependências

Eventos definidos.

### Feature 8.2 — Caixa de notificações

**Objetivo:** organizar modelo e comportamento da lista de notificações.  
**Descrição:** implementar persistência, leitura e status.  
**Critérios de aceitação:** fluxo de notificação compreensível.

#### Tasks

* **ID:** F08.02.01
  * **Título:** Implementar contrato de notificação
  * **Descrição:** criar campos, prioridade e status de leitura.
  * **Prioridade:** Média
  * **Dependências:** F08.01.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** contrato estável.
  * **Definition of Done:** modelo de notificação pronto para implementação.
