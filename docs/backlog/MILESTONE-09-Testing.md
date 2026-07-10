# MILESTONE-09-Testing

## Objetivo

Consolidar a estratégia de testes do Orbit e implementar cobertura obrigatória para os módulos críticos.

## Dependências

* MILESTONE-01-Foundation
* MILESTONE-02-Authentication
* MILESTONE-03-Dashboard
* MILESTONE-04-Transactions
* MILESTONE-05-Import
* MILESTONE-06-Goals
* MILESTONE-07-Investments
* MILESTONE-08-Notifications

## Épico 1 — Estratégia de testes

### Descrição

Implementar a pirâmide de testes e os módulos críticos obrigatórios.

### Objetivo

Garantir confiabilidade contínua e prevenir regressões.

### Dependências

Módulos centrais definidos.

### Feature 9.1 — Plano de cobertura

**Objetivo:** implementar o escopo mínimo de testes.  
**Descrição:** mapear o que deve ser testado por tipo.  
**Critérios de aceitação:** estratégia clara e acionável.

#### Tasks

* **ID:** F09.01.01
  * **Título:** Implementar matriz de testes obrigatórios
  * **Descrição:** criar a lista de módulos que exigem teste unitário, integração e contrato.
  * **Prioridade:** Alta
  * **Dependências:** F03.01.01, F04.01.01, F05.01.01, F06.01.01, F07.01.01, F08.02.01
  * **Arquivos que provavelmente serão modificados:** `docs/backlog/ROADMAP.md`, `ARCHITECTURE.md`
  * **Documentação relacionada:** `ENGINEERING.md`
  * **Critérios de aceitação:** matriz de cobertura publicada.
  * **Definition of Done:** plano de teste oficial consolidado.

* **ID:** F09.01.02
  * **Título:** Implementar padrão de organização de testes
  * **Descrição:** criar onde cada tipo de teste deve residir.
  * **Prioridade:** Média
  * **Dependências:** F09.01.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ENGINEERING.md`
  * **Critérios de aceitação:** estrutura de testes descrita.
  * **Definition of Done:** organização pronta para execução incremental.
