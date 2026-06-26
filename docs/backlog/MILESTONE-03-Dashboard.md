# MILESTONE-03-Dashboard

## Objetivo

Definir e preparar a base de dados agregada e os contratos necessários para o dashboard financeiro.

## Dependências

* MILESTONE-01-Foundation
* MILESTONE-02-Authentication

## Épico 1 — Indicadores

### Descrição

Preparar a lógica de obtenção dos indicadores principais do usuário.

### Objetivo

Disponibilizar saldo, receitas, despesas e evolução mensal com consistência.

### Dependências

Autenticação e base técnica.

### Feature 3.1 — KPIs do dashboard

**Objetivo:** definir os indicadores oficiais.  
**Descrição:** organizar métricas e origem dos dados.  
**Critérios de aceitação:** KPIs descritos e priorizados.

#### Tasks

* **ID:** F03.01.01
  * **Título:** Definir contrato dos KPIs
  * **Descrição:** documentar quais indicadores o dashboard exibirá.
  * **Prioridade:** Alta
  * **Dependências:** F01.03.02
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`, `docs/backlog/ROADMAP.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** indicadores principais explicitados.
  * **Definition of Done:** contrato de dashboard definido.

* **ID:** F03.01.02
  * **Título:** Definir origem dos dados do dashboard
  * **Descrição:** documentar a relação entre transações, contas, categorias e metas.
  * **Prioridade:** Alta
  * **Dependências:** F03.01.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** fontes de dados mapeadas.
  * **Definition of Done:** rastreabilidade de origem estabelecida.

## Épico 2 — Agregações

### Descrição

Preparar agregações que serão usadas pelo frontend e pelos gráficos.

### Objetivo

Reduzir custo de leitura e padronizar consultas.

### Dependências

KPI definidos.

### Feature 3.2 — Consultas agregadas

**Objetivo:** estruturar consultas de leitura do dashboard.  
**Descrição:** definir agregações por período, conta e categoria.  
**Critérios de aceitação:** consultas previsíveis e documentadas.

#### Tasks

* **ID:** F03.02.01
  * **Título:** Definir agregação de saldo
  * **Descrição:** documentar como o saldo atual é calculado.
  * **Prioridade:** Alta
  * **Dependências:** F03.01.02
  * **Arquivos que provavelmente serão modificados:** `app/Services/Dashboard/*`, `app/Repositories/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** cálculo do saldo descrito.
  * **Definition of Done:** lógica de saldo pronta para implementação.

* **ID:** F03.02.02
  * **Título:** Definir agregação por período
  * **Descrição:** documentar a base para evolução mensal e fluxo de caixa.
  * **Prioridade:** Alta
  * **Dependências:** F03.02.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Dashboard/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** período e filtros definidos.
  * **Definition of Done:** consultas de período prontas para uso.

## Épico 3 — Interface do dashboard

### Descrição

Especificar a estrutura visual dos cards, gráficos e filtros.

### Objetivo

Permitir que a implementação frontend siga um contrato estável.

### Dependências

Agregações definidas.

### Feature 3.3 — Layout e estados

**Objetivo:** documentar o comportamento da UI do dashboard.  
**Descrição:** organizar cards, loading, empty state e error state.  
**Critérios de aceitação:** estados de interface previstos.

#### Tasks

* **ID:** F03.03.01
  * **Título:** Definir estados do dashboard
  * **Descrição:** documentar loading, vazio, erro e sucesso.
  * **Prioridade:** Média
  * **Dependências:** F03.01.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`, `src/features/dashboard/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** comportamento visual especificado.
  * **Definition of Done:** estados bem definidos para o frontend.

