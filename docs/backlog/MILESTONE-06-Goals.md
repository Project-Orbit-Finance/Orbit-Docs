# MILESTONE-06-Goals

## Objetivo

Implementar o gerenciamento de metas financeiras com cálculo de progresso e projeção.

## Dependências

* MILESTONE-01-Foundation
* MILESTONE-02-Authentication
* MILESTONE-04-Transactions

## Épico 1 — Modelo de metas

### Descrição

Formalizar as entidades responsáveis por objetivos financeiros.

### Objetivo

Permitir planejamento de curto, médio e longo prazo.

### Dependências

Transações.

### Feature 6.1 — Estrutura da meta

**Objetivo:** definir os dados da meta e suas relações.  
**Descrição:** documentar valor, prazo, progresso e status.  
**Critérios de aceitação:** modelo definido.

#### Tasks

* **ID:** F06.01.01
  * **Título:** Definir contrato de meta
  * **Descrição:** documentar atributos essenciais e opcionais.
  * **Prioridade:** Alta
  * **Dependências:** F04.01.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** campos da meta descritos.
  * **Definition of Done:** contrato pronto para implementação.

## Épico 2 — Cálculos de progresso

### Descrição

Calcular quanto falta, quanto falta aportar e qual a previsão de conclusão.

### Objetivo

Dar visibilidade clara ao usuário sobre o andamento da meta.

### Dependências

Modelo de metas.

### Feature 6.2 — Projeções

**Objetivo:** organizar os cálculos de metas.  
**Descrição:** detalhar fórmula de progresso e projeção.  
**Critérios de aceitação:** cálculos definidos e testáveis.

#### Tasks

* **ID:** F06.02.01
  * **Título:** Definir cálculo de progresso
  * **Descrição:** documentar percentual concluído e saldo faltante.
  * **Prioridade:** Alta
  * **Dependências:** F06.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Goals/*`, `app/Utils/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** fórmula de progresso clara.
  * **Definition of Done:** cálculo pronto para teste unitário.

* **ID:** F06.02.02
  * **Título:** Definir projeção de conclusão
  * **Descrição:** documentar previsão com base em aportes e prazo.
  * **Prioridade:** Média
  * **Dependências:** F06.02.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Goals/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** projeção descrita sem ambiguidade.
  * **Definition of Done:** fórmula documentada para implementação.

