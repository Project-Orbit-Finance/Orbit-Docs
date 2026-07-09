# MILESTONE-01-Foundation

## Objetivo

Estabelecer a base técnica e organizacional do Orbit para permitir desenvolvimento previsível, testável e consistente.

## Dependências

Nenhuma.

## Épico 1 — Estrutura do repositório

### Descrição

Criar a organização inicial do frontend, backend e documentação conforme a arquitetura oficial.

### Objetivo

Garantir que todo o time trabalhe dentro de uma estrutura padronizada.

### Dependências

Nenhuma.

### Feature 1.1 — Estrutura base de pastas

**Objetivo:** definir a estrutura física dos diretórios do projeto.  
**Descrição:** preparar os diretórios para frontend, backend, docs e backlog.  
**Critérios de aceitação:** estrutura oficial disponível e documentada.

#### Tasks

* **ID:** F01.01.01
  * **Título:** Criar estrutura inicial de documentação
  * **Descrição:** organizar `docs/backlog/` e `docs/adr/` de acordo com o roadmap oficial.
  * **Prioridade:** Alta
  * **Dependências:** nenhuma
  * **Arquivos que provavelmente serão modificados:** `docs/backlog/*`, `docs/adr/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`, `ROADMAP.md`
  * **Critérios de aceitação:** diretórios e documentos-base criados e acessíveis.
  * **Definition of Done:** estrutura consistente com a arquitetura oficial e sem arquivos faltantes para o milestone.
  * **Status:** Concluída

* **ID:** F01.01.02
  * **Título:** Organizar diretórios do frontend
  * **Descrição:** preparar a estrutura feature-based prevista na arquitetura.
  * **Prioridade:** Alta
  * **Dependências:** F01.01.01
  * **Arquivos que provavelmente serão modificados:** `src/features/*`, `src/shared/*`, `src/app/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** estrutura base pronta para criação de features.
  * **Definition of Done:** diretórios principais criados sem lógica de negócio.
  * **Status:** Concluída

* **ID:** F01.01.03
  * **Título:** Organizar diretórios do backend
  * **Descrição:** estruturar camadas base do backend para Controllers, Services, Repositories, Requests, Policies, Resources e Models.
  * **Prioridade:** Alta
  * **Dependências:** F01.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Http/Controllers/*`, `app/Services/*`, `app/Repositories/*`, `app/Models/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** camadas base disponíveis para implementação futura.
  * **Definition of Done:** estrutura criada e alinhada ao padrão oficial.
  * **Status:** Concluída

## Épico 2 — Base técnica

### Descrição

Configurar as ferramentas essenciais para desenvolvimento, qualidade e padronização.

### Objetivo

Reduzir risco operacional e permitir desenvolvimento seguro desde o início.

### Dependências

Estrutura base de pastas.

### Feature 1.2 — Qualidade e convenções

**Objetivo:** estabelecer padrões mínimos de consistência.  
**Descrição:** preparar lint, formato, tipos e convenções para ambos os lados da aplicação.  
**Critérios de aceitação:** ambientes preparados para validação automatizada.

#### Tasks

* **ID:** F01.02.01
  * **Título:** Definir convenções de nomenclatura
  * **Descrição:** documentar padrões de nomes de arquivos, classes, hooks e services.
  * **Prioridade:** Alta
  * **Dependências:** F01.01.01
  * **Arquivos que provavelmente serão modificados:** `docs/backlog/ROADMAP.md`, `ARCHITECTURE.md`
  * **Documentação relacionada:** `ENGINEERING.md`
  * **Critérios de aceitação:** padrões claros e rastreáveis.
  * **Definition of Done:** convenções documentadas e utilizáveis por qualquer dev.
  * **Status:** Concluída

* **ID:** F01.02.02
  * **Título:** Definir baseline de validação
  * **Descrição:** registrar a estratégia oficial de lint, testes e build por camada.
  * **Prioridade:** Alta
  * **Dependências:** F01.02.01
  * **Arquivos que provavelmente serão modificados:** `docs/backlog/ROADMAP.md`
  * **Documentação relacionada:** `ENGINEERING.md`
  * **Critérios de aceitação:** baseline de qualidade definido.
  * **Definition of Done:** checklist oficial pronto para uso no desenvolvimento.

## Épico 3 — Contratos iniciais

### Descrição

Formalizar os contratos-base que serão usados por todas as features.

### Objetivo

Evitar retrabalho e divergência entre frontend, backend e documentação.

### Dependências

Base técnica.

### Feature 1.3 — Contratos e tipagem

**Objetivo:** consolidar o padrão de schemas e tipos.  
**Descrição:** definir a abordagem para Zod, DTOs, Resources e respostas da API.  
**Critérios de aceitação:** contratos iniciais compreensíveis e reutilizáveis.

#### Tasks

* **ID:** F01.03.01
  * **Título:** Definir padrão de schemas do frontend
  * **Descrição:** documentar como schemas Zod serão organizados por feature.
  * **Prioridade:** Alta
  * **Dependências:** F01.02.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`, `docs/backlog/ROADMAP.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** padrão de schemas descrito sem ambiguidade.
  * **Definition of Done:** documentação suficiente para implementar formulários com consistência.

* **ID:** F01.03.02
  * **Título:** Definir padrão de resposta da API
  * **Descrição:** documentar formato de sucesso, erro e validação.
  * **Prioridade:** Alta
  * **Dependências:** F01.02.02
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** payloads e erros padronizados.
  * **Definition of Done:** contrato base compreensível para frontend e backend.
