# AGENTS.md

# Instruções para Agentes

Você está trabalhando no projeto Orbit.

Seu objetivo é desenvolver software de qualidade profissional.

## Antes de qualquer alteração

Sempre:

* leia PROJECT.md
* leia README.md
* leia ENGINEERING.md caso exista
* compreenda a arquitetura antes de modificar arquivos

Nunca implemente algo sem entender o contexto.

---

# Regras Gerais

Nunca alterar funcionalidades fora do escopo da tarefa.

Nunca remover código sem justificar.

Nunca quebrar compatibilidade.

Nunca criar soluções temporárias.

Evite comentários desnecessários.

Prefira código autoexplicativo.

---

# Arquitetura

Frontend

Feature Based.

Toda lógica deve permanecer dentro da feature correspondente.

Backend

Controller

↓

Service

↓

Repository

↓

Model

Controllers nunca acessam Models diretamente.

---

# Componentes React

Componentes devem apenas renderizar interface.

Toda lógica deve ficar em hooks ou services.

Componentes grandes devem ser divididos.

---

# Hooks

Hooks devem conter:

* regras de negócio
* chamadas para API
* estados
* validações

Nunca renderizar interface.

---

# Services

Services apenas comunicam com APIs.

Não conter lógica de interface.

---

# Tipagem

Nunca utilizar any.

Sempre preferir tipos explícitos.

Utilizar Zod como fonte única de verdade.

---

# Banco

Nunca realizar consultas desnecessárias.

Criar índices quando apropriado.

Evitar N+1 Query.

Utilizar migrations.

---

# Segurança

Nunca armazenar Access Token em LocalStorage.

Refresh Token deve utilizar HttpOnly Cookie.

Sempre validar entradas.

Nunca confiar no frontend.

Sanitizar todos os dados.

---

# Performance

Evitar renderizações desnecessárias.

Utilizar React Query corretamente.

Memoizar componentes pesados.

Implementar paginação quando necessário.

---

# Código

Preferir funções pequenas.

Preferir composição.

Evitar duplicação.

Seguir SOLID.

Seguir Clean Code.

Seguir DRY.

---

# Testes

Toda funcionalidade nova deve possuir testes.

Após cada implementação:

* executar lint
* executar testes
* corrigir erros encontrados

Não considerar a tarefa concluída caso existam erros.

---

# Documentação

Sempre atualizar:

README

Swagger

Tipos

Documentação técnica

quando necessário.

---

# Commits

Utilizar Conventional Commits.

Exemplos

feat:

fix:

refactor:

test:

docs:

chore:

---

# Quando terminar uma tarefa

Sempre fornecer um resumo contendo:

* arquivos modificados
* decisões técnicas
* impactos
* possíveis melhorias futuras

Nunca finalizar apenas dizendo "feito".
