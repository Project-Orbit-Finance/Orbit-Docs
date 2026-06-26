# PROJECT.md

# Orbit

## Visão Geral

Orbit é uma plataforma web de finanças pessoais desenvolvida para ajudar usuários a compreender, organizar e planejar sua vida financeira.

O objetivo do projeto é demonstrar conhecimentos em engenharia de software, arquitetura de aplicações, segurança, desenvolvimento full stack e modelagem de domínio financeiro.

O projeto deve priorizar qualidade de código, arquitetura limpa, segurança e experiência do usuário antes da quantidade de funcionalidades.

---

# Objetivos

O Orbit deve permitir que um usuário:

* cadastrar uma conta
* autenticar-se com segurança
* importar extratos bancários
* visualizar todas as transações
* categorizar automaticamente despesas e receitas
* editar categorias manualmente
* visualizar gráficos financeiros
* criar metas financeiras
* acompanhar a evolução das metas
* planejar grandes despesas
* simular investimentos
* receber notificações importantes
* obter análises sobre seus hábitos financeiros

---

# Público-alvo

Pessoas que desejam organizar sua vida financeira sem depender de planilhas.

---

# Stack

## Frontend

* React
* TypeScript
* Vite
* TailwindCSS
* shadcn/ui
* React Hook Form
* Zod
* TanStack Query
* Zustand
* Recharts

## Backend

* Laravel
* PHP 8.3
* PostgreSQL
* Laravel Sanctum
* Swagger/OpenAPI

## Infraestrutura

* Vercel
* Railway
* Supabase

## Estrutura de repositórios

O projeto é mantido em três repositórios:

* `orbit-docs` - documentação oficial, backlog, ADRs, arquitetura e contratos
* `orbit-web` - frontend React
* `api` - backend Laravel e banco de dados

---

# Arquitetura

## Frontend

Feature Based Architecture

## Backend

Controller

↓

Service

↓

Repository

↓

Model

Nenhum Controller deve acessar o banco diretamente.

---

# MVP

## Autenticação

* Cadastro
* Login
* Logout
* Refresh Token
* Recuperação de senha

## Dashboard

* Saldo atual
* Receitas
* Despesas
* Gasto do mês
* Evolução mensal

## Importação

Importação de:

* CSV

O sistema deverá validar os arquivos antes da importação.

## Transações

CRUD completo

Campos mínimos

* descrição
* valor
* data
* categoria
* conta
* tipo
* observação

## Categorização

O sistema deverá reconhecer automaticamente padrões.

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

Amazon

↓

Compras

O usuário poderá alterar a categoria manualmente.

As alterações servirão como aprendizado para futuras importações.

## Dashboard Financeiro

Gráficos:

* gastos por categoria
* receitas x despesas
* evolução mensal
* fluxo de caixa
* patrimônio

Filtros

* período
* categoria
* conta

## Metas

Criar metas financeiras.

Exemplo

Comprar notebook

Valor

R$ 7.000

Prazo

10 meses

O sistema deverá calcular:

* quanto falta
* percentual concluído
* economia mensal necessária
* previsão de conclusão

## Planejamento

Permitir planejamento de:

* viagens
* carro
* casa
* notebook
* celular
* qualquer objetivo financeiro

Observação: o planejamento pode ser modelado como parte do fluxo de metas financeiras no MVP, sem criar um módulo separado.

## Simulador

Simular investimentos em:

* CDI
* Tesouro Selic
* LCI
* LCA

Exibir

* rendimento bruto
* rendimento líquido
* imposto
* evolução mensal
* gráfico

## Notificações

Alerta para:

* contas próximas do vencimento
* metas atrasadas
* orçamento excedido
* lembretes personalizados

## Educação Financeira

Área contendo conteúdos educativos.

Inicialmente apenas links e recomendações.

No futuro poderá possuir artigos próprios.

---

# Segurança

* Access Token em memória
* Refresh Token HttpOnly
* HTTPS obrigatório
* Rate Limiting
* Validação server-side
* Sanitização
* Proteção CSRF
* RLS no banco
* Nenhuma informação sensível em logs

Observação: o access token é emitido pelo backend e mantido apenas em memória no frontend. O refresh token permanece em cookie HttpOnly.

---

# Performance

* Lazy Loading
* Suspense
* Code Splitting
* Memoização
* Virtualização de listas grandes

---

# Qualidade

* ESLint
* Prettier
* Vitest
* PHPStan
* Pint

Cobertura mínima

80%

---

# Convenções

Strict Mode

Sem any

Sem código duplicado

Sem funções gigantes

Preferir composição.

---

# Funcionalidades futuras

* Open Finance
* Importação automática
* Aplicativo mobile
* IA para sugestões financeiras
* OCR de comprovantes
* Detecção de assinaturas
* Compartilhamento familiar
* Múltiplas moedas
* Exportação para Excel/PDF

---

# Objetivo final

O Orbit deve parecer um produto real.

Cada funcionalidade deve possuir documentação, testes e arquitetura consistente.
