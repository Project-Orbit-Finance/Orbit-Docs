# MILESTONE-07-Investments

## Objetivo

Implementar o simulador financeiro com módulos independentes por investimento.

## Dependências

* MILESTONE-01-Foundation

## Épico 1 — Estrutura do simulador

### Descrição

Implementar a arquitetura dos cálculos financeiros.

### Objetivo

Garantir isolamento entre interface e lógica matemática.

### Dependências

Foundation.

### Feature 7.1 — Base de cálculo

**Objetivo:** formalizar a regra de cálculo desacoplada da UI.  
**Descrição:** implementar entradas, saídas e contratos dos módulos.  
**Critérios de aceitação:** arquitetura do simulador estabelecida.

#### Tasks

* **ID:** F07.01.01
  * **Título:** Implementar contrato do simulador
  * **Descrição:** criar entradas, saídas e parâmetros gerais.
  * **Prioridade:** Alta
  * **Dependências:** F01.03.02
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** contrato técnico do simulador pronto.
  * **Definition of Done:** módulo pronto para ser implementado por partes.

### Feature 7.2 — Módulos por investimento

**Objetivo:** separar CDI, Selic, LCI e LCA em módulos próprios.  
**Descrição:** implementar responsabilidades de cada cálculo.  
**Critérios de aceitação:** cada investimento possui fronteira própria.

#### Tasks

* **ID:** F07.02.01
  * **Título:** Implementar módulo CDI
  * **Descrição:** criar os parâmetros e a saída do cálculo CDI.
  * **Prioridade:** Média
  * **Dependências:** F07.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Investments/CDI/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** módulo CDI isolado.
  * **Definition of Done:** especificação pronta para implementação.

* **ID:** F07.02.02
  * **Título:** Implementar módulo Tesouro Selic
  * **Descrição:** criar os parâmetros e a saída do cálculo Selic.
  * **Prioridade:** Média
  * **Dependências:** F07.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Investments/Selic/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** módulo Selic isolado.
  * **Definition of Done:** especificação pronta para implementação.

* **ID:** F07.02.03
  * **Título:** Implementar módulos LCI e LCA
  * **Descrição:** criar os parâmetros e as saídas dos cálculos de LCI e LCA.
  * **Prioridade:** Média
  * **Dependências:** F07.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Investments/LCI/*`, `app/Services/Investments/LCA/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** módulos separados definidos.
  * **Definition of Done:** fronteiras de cálculo consolidadas.
