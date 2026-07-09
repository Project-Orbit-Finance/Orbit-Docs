# ENGINEERING.md

# Engenharia de Software

Este documento define todos os padrões técnicos utilizados no projeto Orbit.

Todo agente ou desenvolvedor deve seguir estas diretrizes antes de implementar qualquer funcionalidade.

---

# Filosofia

O objetivo do Orbit não é apenas funcionar.

O projeto deve representar um software que poderia ser colocado em produção.

Toda decisão deve priorizar:

* Legibilidade
* Segurança
* Escalabilidade
* Manutenibilidade
* Performance
* Experiência do usuário

Sempre prefira qualidade à velocidade de implementação.

---

# Arquitetura Geral

Frontend

React

↓

Hooks

↓

Services

↓

API

Backend

Controller

↓

Service

↓

Repository

↓

Model

↓

Database

Nenhuma camada pode acessar diretamente uma camada que não seja sua dependência imediata.

---

# Estrutura do Frontend

Utilizar arquitetura Feature Based.

```
src/

features/

auth/

components/

hooks/

services/

types.ts

transactions/

dashboard/

goals/

calculator/

notifications/

education/

shared/

components/

hooks/

lib/

services/

types/

constants/

utils/
```

Não criar componentes específicos de uma feature dentro de `shared`.

Tudo que estiver em `shared` deve ser reutilizável.

---

# Convenções de Nomenclatura

## Frontend

### Diretórios

* `app/` para composição global da aplicação
* `features/` para domínios de negócio isolados
* `shared/` para código realmente reutilizável

### Arquivos e pastas

* componentes em PascalCase quando representarem arquivos específicos de componente
* hooks com prefixo `use`
* services com sufixo `Service`
* schemas com o nome da feature ou da entidade
* types com o nome do domínio ou da feature

### Exemplos

* `useTransactions`
* `useGoals`
* `TransactionService`
* `GoalService`
* `transaction.schema.ts`
* `goal.types.ts`

## Backend

### Diretórios

* `Controllers` para entradas HTTP
* `Services` para regras de negócio
* `Repositories` para persistência
* `Requests` para validação de entrada
* `Policies` para autorização
* `Resources` para resposta da API
* `Models` para entidades persistidas

### Classes

* Controllers devem usar o sufixo `Controller`
* Services devem usar o sufixo `Service`
* Repositories devem usar o sufixo `Repository`
* Requests devem usar o sufixo `Request`
* Policies devem usar o sufixo `Policy`
* Resources devem usar o sufixo `Resource`
* Models devem usar nomes no singular

### Exemplos

* `AuthController`
* `TransactionService`
* `TransactionRepository`
* `StoreTransactionRequest`
* `TransactionPolicy`
* `TransactionResource`
* `Transaction`

## Regras gerais

* nomes devem ser descritivos e consistentes
* evitar abreviações não óbvias
* manter singular para entidades e plural para coleções ou pastas de agrupamento
* manter o mesmo termo para o mesmo conceito em todo o projeto
* não usar nomes genéricos como `Helper` ou `Manager` sem justificativa clara

---

# Componentes React

Cada componente deve possuir apenas uma responsabilidade.

Evitar componentes maiores que 150 linhas.

Caso um componente cresça demais, dividi-lo.

Componentes nunca devem conter regras de negócio.

Componentes apenas:

* recebem props
* renderizam interface
* disparam eventos

---

# Custom Hooks

Hooks devem concentrar a orquestração das features no frontend.

Regras de negócio puras e cálculos determinísticos devem ficar em módulos de domínio, `utils/` ou services específicos, conforme a arquitetura oficial.

Exemplo:

```
useTransactions()

useGoals()

useBudget()

useInvestmentCalculator()
```

Hooks são responsáveis por:

* gerenciamento de estado
* comunicação com API
* cálculos
* transformação de dados
* filtros
* paginação

Quando um cálculo fizer parte da regra central do produto, ele deve ser isolado para facilitar testes e reuso, evitando acoplamento excessivo ao hook.

---

# Services

Services apenas comunicam com APIs.

Nunca acessar fetch ou axios diretamente em componentes.

Exemplo

```
TransactionService

GoalService

AuthService

InvestmentService
```

---

# Estado Global

Utilizar Zustand apenas para estados globais.

Exemplos:

* usuário autenticado
* tema
* configurações
* notificações

Nunca utilizar Zustand para cache da API.

---

# Estado da API

Toda comunicação com servidor deve utilizar TanStack Query.

Utilizar:

* cache
* retries
* invalidation
* optimistic updates quando apropriado

Nunca fazer fetch manual se React Query puder resolver.

---

# Formulários

Todo formulário deve utilizar

React Hook Form

*

Zod

O schema do Zod deve ser reutilizado sempre que possível.

Nunca duplicar validações.

---

# Organização do Código

Uma função deve fazer apenas uma coisa.

Preferir funções pequenas.

Evitar arquivos gigantes.

Evitar condicionais profundamente aninhadas.

Preferir early return.

---

# TypeScript

Configuração obrigatória

```
strict: true
```

Nunca utilizar:

```
any
```

Preferir:

* unknown
* generics
* interfaces
* utility types

Todo retorno deve possuir tipo explícito quando melhorar a legibilidade.

---

# Backend

Estrutura

```
Controllers

Services

Repositories

DTOs

Requests

Policies

Resources

Models
```

Controller

Responsável apenas por:

* receber request
* validar
* chamar service
* retornar response

Toda regra de negócio pertence aos Services.

Toda consulta ao banco pertence aos Repositories.

---

# Banco de Dados

Utilizar migrations.

Nunca alterar migrations já executadas.

Criar novas migrations.

Criar índices sempre que necessário.

Evitar SELECT *.

Evitar consultas N+1.

Utilizar eager loading.

Normalizar dados quando apropriado.

---

# Segurança

Nunca armazenar Access Token no LocalStorage.

Access Token permanece apenas em memória.

Refresh Token deve utilizar HttpOnly Cookie.

Sempre validar dados no backend.

Nunca confiar apenas na validação do frontend.

Sanitizar entradas.

Rate Limit em autenticação.

Proteção contra:

* SQL Injection
* XSS
* CSRF
* Mass Assignment

Nunca registrar informações sensíveis em logs.

---

# Categorização Automática

A categorização deve ser baseada inicialmente em regras.

Exemplo

Uber

↓

Transporte

iFood

↓

Alimentação

Netflix

↓

Assinaturas

Posteriormente o sistema poderá utilizar aprendizado baseado nas alterações realizadas pelo usuário.

Sempre permitir edição manual.

---

# Calculadora Financeira

Toda lógica financeira deve ficar isolada.

Criar um módulo específico.

Nunca misturar cálculos financeiros com componentes.

Cada investimento deve possuir seu próprio serviço.

Exemplo

```
calculateCDI()

calculateSelic()

calculateLCI()

calculateLCA()
```

Os cálculos devem ser facilmente testáveis.

---

# Testes

Frontend

Vitest

Backend

PHPUnit

Todo cálculo financeiro deve possuir testes.

Toda regra de categorização deve possuir testes.

Cobertura mínima

80%

---

# Performance

Utilizar

React.lazy

Suspense

Memo

useCallback

useMemo

Virtualização quando listas ultrapassarem algumas centenas de registros.

Paginação para consultas grandes.

Nunca renderizar gráficos desnecessariamente.

---

# Qualidade

Executar antes de cada commit

Frontend

```
npm run lint

npm run test

npm run build
```

Backend

```
php artisan test

php artisan pint

phpstan analyse
```

Nenhum commit deve ser realizado com erros.

---

# Git

Branch principal

main

Branches de desenvolvimento

feature/nome-da-feature

fix/nome-do-fix

refactor/nome

Commits

Conventional Commits

Exemplo

```
feat(auth): implement JWT authentication

fix(import): correct CSV parser

refactor(transactions): simplify categorization

docs(readme): update architecture

test(calculator): add CDI tests
```

---

# Documentação

Sempre manter atualizados

README

Swagger

PROJECT.md

ENGINEERING.md

ADRs

Sempre documentar decisões importantes.

---

# Critérios de Aceitação

Toda funcionalidade será considerada pronta apenas quando:

* Implementada
* Testada
* Documentada
* Sem erros de lint
* Sem erros de build
* Sem vulnerabilidades conhecidas
* Integrada ao restante do sistema
* Revisada quanto à arquitetura

Caso algum destes itens não seja atendido, a tarefa não deve ser considerada concluída.

---

# Objetivo Final

Todo código produzido deve ser legível, reutilizável e preparado para evolução futura.

Em caso de dúvida entre uma solução rápida e uma solução sustentável, priorize a solução sustentável.

O Orbit deve servir como um projeto de referência em arquitetura, qualidade de código e boas práticas de desenvolvimento Full Stack.
