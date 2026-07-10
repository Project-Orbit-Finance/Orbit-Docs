# MILESTONE-02-Authentication

## Objetivo

Implementar o fluxo seguro de autenticação, autorização e sessão do Orbit.

## Dependências

* MILESTONE-01-Foundation

## Épico 1 — Cadastro e login

### Descrição

Criar os fluxos públicos de criação de conta e acesso ao sistema.

### Objetivo

Permitir que o usuário entre no sistema com segurança e previsibilidade.

### Dependências

Foundation.

### Feature 2.1 — Cadastro

**Objetivo:** permitir criação de conta.  
**Descrição:** incluir validação, persistência e resposta adequada.  
**Critérios de aceitação:** usuário consegue criar conta com dados válidos.

#### Tasks

* **ID:** F02.01.01
  * **Título:** Definir request de cadastro
  * **Descrição:** estruturar validações mínimas de nome, e-mail e senha.
  * **Prioridade:** Alta
  * **Status:** Concluída
  * **Dependências:** F01.03.02
  * **Arquivos que provavelmente serão modificados:** `app/Http/Requests/*`, `docs/backlog/ROADMAP.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** regras de entrada claras.
  * **Definition of Done:** validações documentadas e prontas para uso.

* **ID:** F02.01.02
  * **Título:** Criar service de cadastro
  * **Descrição:** registrar usuário e aplicar regras de criação.
  * **Prioridade:** Alta
  * **Status:** Concluída
  * **Dependências:** F02.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Auth/*`, `app/Repositories/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** criação de usuário orquestrada pelo backend.
  * **Definition of Done:** caso de uso documentado e consistente.

### Feature 2.2 — Login e logout

**Objetivo:** permitir entrada e saída da sessão.  
**Descrição:** processar credenciais, emitir tokens e encerrar sessão.  
**Critérios de aceitação:** login e logout funcionam com retorno adequado.

#### Tasks

* **ID:** F02.02.01
  * **Título:** Definir endpoint de login
  * **Descrição:** estruturar request e response do login.
  * **Prioridade:** Alta
  * **Status:** Concluída
  * **Dependências:** F02.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Http/Controllers/Auth/*`, `app/Http/Requests/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** contrato do login definido.
  * **Definition of Done:** fluxo especificado para implementação.

* **ID:** F02.02.02
  * **Título:** Definir endpoint de logout
  * **Descrição:** estruturar invalidação de sessão e limpeza de cookie.
  * **Prioridade:** Alta
  * **Status:** Concluída
  * **Dependências:** F02.02.01
  * **Arquivos que provavelmente serão modificados:** `app/Http/Controllers/Auth/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** regra de logout documentada.
  * **Definition of Done:** comportamento de encerramento claro.

## Épico 2 — Refresh e recuperação

### Descrição

Garantir continuidade segura da sessão e recuperação de acesso.

### Objetivo

Evitar reautenticações desnecessárias e suportar recuperação de senha.

### Dependências

Login e logout.

### Feature 2.3 — Refresh token

**Objetivo:** renovar sessão sem expor token ao frontend.  
**Descrição:** descrever o ciclo de refresh e proteção.  
**Critérios de aceitação:** fluxo de renovação formalizado.

#### Tasks

* **ID:** F02.03.01
  * **Título:** Definir fluxo de refresh
  * **Descrição:** documentar renovação de access token via cookie HttpOnly.
  * **Prioridade:** Alta
  * **Status:** Concluída
  * **Dependências:** F02.02.01
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`, `app/Http/Controllers/Auth/*`
  * **Documentação relacionada:** `ADR-001.md`
  * **Critérios de aceitação:** refresh detalhado passo a passo.
  * **Definition of Done:** risco de sessão documentado e mitigado.

### Feature 2.4 — Recuperação de senha

**Objetivo:** permitir reset seguro de senha.  
**Descrição:** definir solicitação, token e redefinição final.  
**Critérios de aceitação:** fluxo claro sem expor contas existentes.

#### Tasks

* **ID:** F02.04.01
  * **Título:** Definir fluxo de solicitação de reset
  * **Descrição:** estruturar request para solicitação de recuperação.
  * **Prioridade:** Alta
  * **Dependências:** F02.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Http/Requests/*`, `app/Http/Controllers/Auth/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** fluxo de solicitação definido.
  * **Definition of Done:** etapa inicial do reset documentada.

* **ID:** F02.04.02
  * **Título:** Definir fluxo de redefinição
  * **Descrição:** documentar validação do token e atualização da senha.
  * **Prioridade:** Alta
  * **Dependências:** F02.04.01
  * **Arquivos que provavelmente serão modificados:** `app/Http/Controllers/Auth/*`, `app/Services/Auth/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** redefinição de senha claramente descrita.
  * **Definition of Done:** fluxo final pronto para implementação.
