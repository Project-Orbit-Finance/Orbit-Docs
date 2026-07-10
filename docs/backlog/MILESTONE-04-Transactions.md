# MILESTONE-04-Transactions

## Objetivo

Implementar a base de gerenciamento de transações do Orbit, incluindo persistência, edição e categorização manual.

## Dependências

* MILESTONE-01-Foundation
* MILESTONE-02-Authentication

## Épico 1 — Modelo de transações

### Descrição

Implementar a entidade de transação e seus relacionamentos essenciais.

### Objetivo

Estabelecer a fonte da verdade para entradas e saídas financeiras.

### Dependências

Autenticação e base técnica.

### Feature 4.1 — Estrutura do domínio

**Objetivo:** detalhar atributos e relações da transação.  
**Descrição:** implementar campos, vínculos e regras base.  
**Critérios de aceitação:** contrato do domínio de transação documentado.

#### Tasks

* **ID:** F04.01.01
  * **Título:** Implementar contrato de transação
  * **Descrição:** criar os campos obrigatórios, opcionais e deriváveis.
  * **Prioridade:** Alta
  * **Dependências:** F01.03.02
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** campos e regras descritos.
  * **Definition of Done:** contrato suficiente para modelagem técnica.

* **ID:** F04.01.02
  * **Título:** Implementar relacionamento com categoria e conta
  * **Descrição:** criar como uma transação referencia conta e categoria.
  * **Prioridade:** Alta
  * **Dependências:** F04.01.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ADR-001.md`
  * **Critérios de aceitação:** relacionamentos sem ambiguidade.
  * **Definition of Done:** base relacional documentada.

## Épico 2 — CRUD de transações

### Descrição

Implementar os fluxos de listagem, criação, edição e exclusão lógica.

### Objetivo

Permitir que o usuário gerencie suas transações manualmente.

### Dependências

Modelo de transações.

### Feature 4.2 — Operações básicas

**Objetivo:** organizar as operações principais.  
**Descrição:** estruturar contrato de API e comportamento.  
**Critérios de aceitação:** operações bem definidas.

#### Tasks

* **ID:** F04.02.01
  * **Título:** Implementar contrato de listagem
  * **Descrição:** criar filtros, paginação e ordenação da listagem.
  * **Prioridade:** Alta
  * **Dependências:** F04.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Http/Controllers/Transaction/*`, `app/Repositories/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** listagem com filtros definida.
  * **Definition of Done:** contrato de consulta pronto.

* **ID:** F04.02.02
  * **Título:** Implementar contrato de criação e edição
  * **Descrição:** criar os payloads de criação e atualização.
  * **Prioridade:** Alta
  * **Dependências:** F04.02.01
  * **Arquivos que provavelmente serão modificados:** `app/Http/Requests/*`, `app/Services/Transaction/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** fluxos de escrita claros.
  * **Definition of Done:** comportamento de mutação definido.

## Épico 3 — Categorização manual

### Descrição

Permitir que o usuário corrija categorias e gere regras personalizadas.

### Objetivo

Fechar o ciclo de aprendizado do sistema no MVP.

### Dependências

CRUD de transações.

### Feature 4.3 — Edição de categoria

**Objetivo:** implementar a atualização manual de categorias.  
**Descrição:** registrar regra personalizada a partir da edição.  
**Critérios de aceitação:** correção manual gera efeito persistente.

#### Tasks

* **ID:** F04.03.01
  * **Título:** Implementar fluxo de correção manual
  * **Descrição:** criar como a alteração de categoria gera nova regra.
  * **Prioridade:** Alta
  * **Dependências:** F04.02.02
  * **Arquivos que provavelmente serão modificados:** `app/Services/Categorization/*`, `app/Repositories/*`
  * **Documentação relacionada:** `ADR-001.md`, `ARCHITECTURE.md`
  * **Critérios de aceitação:** reaprendizado personalizado descrito.
  * **Definition of Done:** regra de retroalimentação documentada.
