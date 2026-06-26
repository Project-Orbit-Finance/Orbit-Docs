# ARCHITECTURE.md

# Arquitetura Oficial do Orbit

Este documento define a arquitetura oficial do Orbit e deve ser considerado a referência principal para implementação.

Ele consolida:

* `PROJECT.md`
* `ENGINEERING.md`
* `AGENTS.md`
* `ADR-001.md`

Se houver conflito entre documentos, a ADR aceita prevalece.

## Estrutura de repositórios

O ecossistema Orbit é dividido em três repositórios:

* `orbit-docs` - documentação oficial, backlog, ADRs e contratos
* `orbit-web` - aplicação frontend
* `api` - backend Laravel

Este documento vive em `orbit-docs` e descreve os contratos e a arquitetura de todo o sistema.

---

# 1. Visão Geral

## Objetivo do sistema

Orbit é uma plataforma web de finanças pessoais para organizar contas, importar transações, categorizar despesas e receitas, acompanhar metas, visualizar indicadores financeiros e apoiar decisões do usuário com clareza e segurança.

O foco do sistema é fornecer uma base robusta, realista e escalável para uso em produção, com domínio financeiro bem modelado e experiência consistente.

## Princípios arquiteturais

1. Separação clara de responsabilidades.
   - Cada camada deve executar apenas o seu papel.
   - Regras de negócio não devem ficar em componentes visuais.

2. Feature-Based no frontend.
   - Cada domínio funcional deve ser isolado em sua própria feature.
   - O compartilhamento entre features deve ocorrer apenas quando houver real reutilização.

3. Backend em camadas.
   - Controller → Service → Repository → Model.
   - Controllers não devem acessar o banco diretamente.

4. Regra de negócio no backend sempre que envolver fonte da verdade.
   - Autenticação, autorização, importação, persistência e validação crítica devem estar no servidor.

5. Regras puras e testáveis.
   - Cálculos financeiros, categorização determinística e normalizações devem ser implementados de forma isolada para facilitar testes.

6. Segurança por padrão.
   - Token de acesso em memória.
   - Refresh token em cookie HttpOnly.
   - Validação server-side obrigatória.
   - Sanitização de entradas.

7. Evolução incremental.
   - O MVP privilegia soluções simples e robustas.
   - Funcionalidades futuras devem encaixar sem reestruturação completa.

## Tecnologias escolhidas

### Frontend

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

### Backend

* Laravel
* PHP 8.3
* PostgreSQL
* Laravel Sanctum
* Swagger/OpenAPI

### Infraestrutura

* Vercel para frontend
* Railway para backend
* Supabase como infraestrutura de banco PostgreSQL, quando aplicável

## Motivos das escolhas

1. React + TypeScript.
   - Boa ergonomia para interfaces ricas e domínio complexo.
   - TypeScript reduz ambiguidades em fluxos financeiros.

2. Vite.
   - Build rápido, bom DX e integração simples com React.

3. TailwindCSS + shadcn/ui.
   - Permitem UI consistente, composável e rápida de evoluir.
   - Reduzem custo de manutenção de design system inicial.

4. Zod + React Hook Form.
   - Unifica validação, schema e tipagem de formulários.
   - Facilita reuso entre frontend e contratos.

5. TanStack Query.
   - Resolve cache, invalidação, retries e sincronização com o servidor.
   - Evita fetch manual disperso pela aplicação.

6. Zustand.
   - Adequado para estado global pequeno e bem definido, como tema, sessão em memória e preferências gerais.

7. Laravel + PostgreSQL.
   - Laravel fornece produtividade, segurança e organização de backend.
   - PostgreSQL é apropriado para domínio relacional e consultas analíticas.

8. Sanctum.
   - Adequado para autenticação com cookies seguros e integração com Laravel.
   - O access token é mantido em memória no frontend e o refresh token permanece em HttpOnly cookie.

9. Recharts.
   - Suficiente para dashboards financeiros do MVP sem introduzir complexidade desnecessária.

---

# 2. Arquitetura Geral

## Fluxo de camadas

Frontend
↓
API
↓
Controllers
↓
Services
↓
Repositories
↓
Models
↓
PostgreSQL

## Papel de cada camada

### Frontend

Responsável por:

* renderização da interface
* navegação
* formulários
* estado de UI
* orquestração de chamadas via hooks
* exibição de feedback para o usuário

O frontend não acessa banco de dados diretamente e não contém a fonte da verdade para regras críticas.

### API

É a fronteira HTTP entre frontend e backend.

Responsável por:

* autenticação
* autorização
* validação de requests
* exposição de endpoints
* retorno padronizado de respostas e erros

### Controllers

Responsáveis por:

* receber request
* validar com `Request` classes quando aplicável
* chamar o `Service` correto
* devolver `Resource` ou payload estruturado

Controllers não devem conter regra de negócio nem consulta direta ao banco.

### Services

Responsáveis por:

* regras de negócio
* orquestração de casos de uso
* decisões de domínio
* coordenação entre repositórios e outras dependências

### Repositories

Responsáveis por:

* consultas ao banco
* persistência
* filtros e paginação
* operações de leitura e escrita com foco em dados

Repositories não devem conter regra de negócio.

### Models

Representam as entidades persistidas e relações do domínio.

Devem permanecer focados em estrutura e relacionamento, não em orquestração complexa.

### PostgreSQL

Banco relacional oficial do sistema.

Deve suportar:

* integridade referencial
* índices apropriados
* auditoria
* consultas analíticas
* evolução futura do domínio

---

# 3. Frontend

## Organização das features

O frontend deve seguir Feature-Based Architecture.

Cada feature deve concentrar tudo que pertence ao seu domínio:

* `components`
* `hooks`
* `services`
* `schemas`
* `types`
* `utils` específicos da feature, quando necessário

Exemplo de features do Orbit:

* `auth`
* `dashboard`
* `transactions`
* `goals`
* `calculator`
* `notifications`
* `education`
* `settings`

## Organização das pastas

Estrutura conceitual recomendada:

```text
src/
  features/
    auth/
      components/
      hooks/
      services/
      schemas/
      types/
    dashboard/
    transactions/
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
  app/
    routes/
    providers/
    store/
    layouts/
```

## Regras de organização

1. Componentes específicos de feature não devem ir para `shared`.
2. `shared` deve conter apenas código realmente reutilizável.
3. Regras de negócio não devem ficar em componentes.
4. Hooks podem conter orquestração, estado e transformação de dados.
5. Services do frontend devem apenas comunicar com a API.

## Componentes

Componentes devem:

* receber props
* renderizar interface
* disparar eventos

Componentes não devem:

* buscar dados diretamente
* conter regra de negócio complexa
* executar validação de domínio

Componentes grandes devem ser divididos por responsabilidade.

## Hooks

Hooks são a camada de orquestração do frontend.

Eles podem conter:

* estado local
* estado derivado
* chamadas de API via React Query
* transformação de dados
* filtros
* paginação
* coordenação de formulário e efeitos

Os hooks são o lugar apropriado para reunir a lógica de uma feature, desde que a regra não seja puramente visual.

## Services

Services do frontend devem ser finos e previsíveis.

Função principal:

* encapsular chamadas HTTP
* centralizar endpoints
* padronizar payloads

Services não devem:

* manipular interface
* conter regra de apresentação
* carregar estado

## Estado global

Zustand deve ser usado apenas para estado global pequeno e estável, como:

* usuário autenticado em memória
* tema
* preferências gerais
* estado compartilhado de UI

Não usar Zustand como cache de servidor.

Todo dado vindo da API deve ser gerenciado por TanStack Query.

## React Query

TanStack Query é a camada oficial para estado de servidor.

Deve ser usado para:

* leitura de dados
* mutações
* cache
* invalidação
* retries
* optimistic updates quando apropriado

Benefícios:

* elimina fetch manual disperso
* melhora consistência de cache
* facilita atualização após mutações

## Autenticação no frontend

O frontend deve manter apenas o access token em memória, se necessário para requisições.

O refresh token permanece em cookie HttpOnly, invisível ao JavaScript.

Fluxo esperado:

1. usuário autentica
2. backend valida e emite a sessão/token conforme decisão oficial
3. frontend armazena apenas o mínimo necessário em memória
4. requisições autenticadas usam interceptor ou wrapper de API
5. em caso de expiração, o frontend aciona refresh
6. se o refresh falhar, o usuário é redirecionado ao login

## Roteamento

O roteamento deve ser orientado por features e por níveis de acesso.

Regras:

* rotas públicas: login, cadastro, recuperação de senha
* rotas protegidas: dashboard, transações, metas, simulador, notificações
* redirecionamento automático quando não houver sessão válida

Layouts devem ser separados para:

* área autenticada
* área pública

## Gerenciamento de formulários

Todos os formulários devem usar React Hook Form com Zod.

Vantagens:

* validação declarativa
* tipagem consistente
* integração natural com UI
* schema reaproveitável entre formulário e regras

## Validação

Zod é a fonte única de verdade para validação no frontend.

Deve ser usado para:

* cadastro
* login
* recuperação de senha
* transações
* metas
* importação
* simulador

O backend sempre valida novamente.

## Gráficos

Gráficos devem ficar na feature de dashboard, com componentes especializados para:

* receitas x despesas
* gastos por categoria
* evolução mensal
* fluxo de caixa
* patrimônio

Os componentes gráficos devem receber dados já preparados pelo hook.

## Tratamento de erros

O frontend deve tratar erros em três níveis:

1. Erro de validação local.
2. Erro de API.
3. Erro inesperado de runtime.

Boas práticas:

* mensagens claras ao usuário
* fallback visual em telas críticas
* estados vazios explícitos
* tratamento de `loading`, `success` e `error`

## Fluxo completo de uma requisição

1. O componente dispara uma ação.
2. O hook executa a mutação ou query.
3. O service faz a chamada HTTP.
4. A API do backend processa a requisição.
5. O backend valida, aplica regra de negócio e persiste.
6. O backend retorna payload estruturado.
7. O hook atualiza o estado de servidor via React Query.
8. O componente renderiza o resultado.

---

# 4. Backend

## Controllers

Controllers são a porta de entrada HTTP.

Responsabilidades:

* receber request
* extrair dados necessários
* validar com `Request` classes
* chamar Services
* retornar responses ou resources

Controllers devem ser pequenos e previsíveis.

## Services

Services concentram a regra de negócio.

Responsabilidades:

* autenticação
* coordenação de importação
* categorização
* cálculo de metas
* cálculo de indicadores
* orquestração de notificações
* simulação financeira

Services devem representar casos de uso e não apenas “camadas intermediárias”.

## Repositories

Repositories concentram a persistência.

Responsabilidades:

* buscar dados
* criar registros
* atualizar registros
* consultar com filtros
* paginação
* agregações persistidas quando necessário

Repositories devem ser previsíveis e não carregar regra de domínio.

## Models

Models representam o mapeamento das entidades do domínio.

Responsabilidades:

* definir relacionamentos
* expor casts e atributos básicos
* refletir a estrutura persistida

Models não devem concentrar lógica complexa de negócio.

## Requests

Requests devem conter validação de entrada e autorização contextual quando aplicável.

Responsabilidades:

* validar payload
* restringir formatos inválidos
* padronizar mensagens de erro

## Policies

Policies controlam autorização em nível de recurso.

Devem responder perguntas como:

* este usuário pode ver esta transação?
* este usuário pode editar esta meta?
* este usuário pode excluir esta importação?

## Resources

Resources controlam a forma de resposta da API.

Responsabilidades:

* padronizar payload
* evitar vazamento de campos internos
* manter contratos estáveis

## Middleware

Middleware deve tratar preocupações transversais:

* autenticação
* proteção CSRF quando aplicável
* rate limiting
* auditoria técnica
* headers de segurança

Middleware não deve carregar regra de negócio.

---

# 5. Banco de Dados

## Entidades principais

As entidades abaixo formam a base do domínio oficial:

* `users`
* `accounts`
* `transactions`
* `categories`
* `import_batches`
* `import_items`
* `goal_plans`
* `goals`
* `notifications`
* `categorization_rules`
* `audit_logs`
* `password_reset_tokens` ou estrutura equivalente conforme Laravel

## Relacionamentos

1. Usuário possui muitas contas.
2. Usuário possui muitas transações.
3. Conta possui muitas transações.
4. Categoria pode ser compartilhada globalmente ou personalizada por usuário, dependendo da regra de domínio.
5. Import batch possui muitos import items.
6. Import item pode gerar uma transação persistida.
7. Meta pode estar associada a um plano de metas.
8. Notificações pertencem ao usuário.
9. Regras de categorização pertencem ao usuário ou ao escopo global, conforme o tipo.

## Estratégia de normalização

O banco deve ser normalizado até o ponto em que:

* reduza redundância
* preserve integridade
* facilite rastreabilidade
* mantenha performance aceitável para consultas analíticas

O desenho deve evitar duplicação desnecessária de categorias, contas e importações.

Quando houver necessidade de performance de leitura, podem existir tabelas derivadas ou agregadas, mas a fonte da verdade deve permanecer normalizada.

## Índices

Índices devem ser criados para os caminhos de consulta mais frequentes, como:

* `user_id`
* `account_id`
* `category_id`
* `transaction_date`
* `import_batch_id`
* `status`
* combinações entre usuário e data

Índices compostos devem ser definidos conforme os filtros reais dos dashboards e listas.

## Chaves estrangeiras

Chaves estrangeiras devem ser usadas sempre que houver relacionamento persistente entre entidades.

Isso garante:

* integridade referencial
* consistência de domínio
* prevenção de órfãos

## Política de exclusão

Para domínio financeiro, a exclusão física deve ser evitada sempre que houver impacto de auditoria.

Política recomendada:

* `soft delete` para registros sensíveis ou historicamente relevantes
* exclusão física apenas quando não houver impacto de rastreabilidade

Transações, importações, regras e eventos relevantes devem preservar histórico.

## Estratégia de auditoria

Cada tabela deve possuir timestamps.

Além disso, eventos relevantes devem ser auditados, como:

* cadastro
* login
* refresh de sessão
* importação
* alteração manual de categoria
* criação e atualização de metas
* exclusões lógicas

Regras:

* não registrar dados financeiros sensíveis em logs
* registrar o contexto do evento sem expor conteúdo sensível
* manter trilha suficiente para rastreabilidade

---

# 6. Fluxo de Autenticação

## Cadastro

1. Usuário envia nome, e-mail e senha.
2. Backend valida formato e política de senha.
3. Backend verifica duplicidade de e-mail.
4. Backend cria o usuário.
5. Backend emite a sessão/token conforme a estratégia oficial.
6. Frontend redireciona para a área autenticada.

## Login

1. Usuário envia e-mail e senha.
2. Backend valida credenciais.
3. Backend aplica rate limiting.
4. Backend gera acesso e refresh.
5. Refresh token fica em cookie HttpOnly.
6. Access token permanece em memória quando necessário.
7. Frontend atualiza o estado de sessão.

## Refresh

1. Access token expira.
2. Frontend intercepta falha de autenticação.
3. Frontend chama endpoint de refresh.
4. Cookie HttpOnly é enviado automaticamente.
5. Backend valida refresh token.
6. Backend emite novo access token e renova o refresh quando aplicável.
7. Frontend retoma a operação original.

## Logout

1. Usuário solicita logout.
2. Backend invalida a sessão/token.
3. Cookie HttpOnly é removido.
4. Frontend limpa estado de sessão em memória.
5. Usuário é redirecionado para a tela pública.

## Recuperação de senha

1. Usuário informa e-mail.
2. Backend valida existência da conta sem expor informação excessiva.
3. Backend gera token de recuperação.
4. Backend envia instrução por e-mail.
5. Usuário acessa o link de redefinição.
6. Backend valida token e janela de tempo.
7. Usuário define nova senha.
8. Backend invalida tokens anteriores e, se aplicável, sessões ativas.

## Fluxo detalhado

O fluxo oficial deve proteger contra:

* brute force
* CSRF no refresh
* vazamento de token em storage inseguro
* enumeração de usuários na recuperação de senha

---

# 7. Fluxo de Importação

## Upload

1. Usuário envia arquivo CSV.
2. Frontend valida extensão, tamanho e formato básico.
3. Arquivo é enviado ao backend em endpoint dedicado.

## Validação

1. Backend confirma tipo do arquivo.
2. Backend verifica estrutura mínima esperada.
3. Backend rejeita arquivos inválidos antes do processamento completo.

## Parser

1. O backend interpreta o CSV.
2. Cada linha é convertida em item importável.
3. Erros de parsing são registrados por linha quando possível.

## Normalização

1. Datas são padronizadas.
2. Valores monetários são convertidos para formato interno.
3. Descrições são limpas e normalizadas.
4. Contas e categorias são resolvidas de forma determinística.

## Categorização

1. Cada item importado passa pela engine de categorização.
2. Regras do usuário têm prioridade.
3. Regras globais vêm em seguida.
4. Caso não exista correspondência, o item fica como “não categorizado” ou categoria padrão definida.

## Persistência

1. O sistema grava o lote de importação.
2. Os itens importados são persistidos de forma rastreável.
3. Transações geradas são vinculadas ao lote.
4. O sistema evita duplicidade por meio de chaves de deduplicação ou critérios equivalentes.

## Dashboard

1. Após a persistência, os dados passam a alimentar consultas agregadas.
2. O dashboard reflete o novo saldo, despesas, receitas e categorias.

## Regras importantes

* Importação deve ser idempotente sempre que possível.
* Erros devem ser explicáveis ao usuário.
* O lote importado deve poder ser auditado posteriormente.

---

# 8. Categorização

## Funcionamento das regras

A categorização do MVP é determinística e baseada em palavras-chave e padrões definidos.

Exemplo:

* `Uber` → `Transporte`
* `iFood` → `Alimentação`
* `Netflix` → `Assinaturas`

## Prioridade das regras

A prioridade deve seguir esta ordem:

1. Regra personalizada do usuário
2. Regra global do sistema
3. Correspondência por palavra-chave
4. Fallback para categoria manual ou não classificada

## Personalização por usuário

Quando o usuário corrige uma categoria manualmente, o sistema deve registrar essa decisão como uma nova regra personalizada para aquele usuário.

Isso permite:

* melhorar a importação futura
* adaptar a categorização ao comportamento do usuário
* preservar preferências individuais

## Estratégia de evolução futura

No MVP não haverá machine learning.

Evolução futura possível:

* pontuação por confiança
* normalização linguística avançada
* aprendizado supervisionado baseado nas correções
* regras por instituição financeira

Qualquer evolução deve preservar o comportamento determinístico base.

---

# 9. Dashboard

## Origem dos dados

O dashboard deve ser alimentado por:

* transações persistidas
* contas do usuário
* categorias
* metas
* importações

## Consultas

As consultas devem ser orientadas por:

* usuário autenticado
* período
* conta
* categoria
* status

## Agregações

O backend deve consolidar dados para:

* saldo atual
* receitas
* despesas
* gasto do mês
* evolução mensal
* fluxo de caixa
* patrimônio

## Gráficos

Os gráficos oficiais são:

* gastos por categoria
* receitas x despesas
* evolução mensal
* fluxo de caixa
* patrimônio

## Filtros

Filtros obrigatórios:

* período
* categoria
* conta

## Indicadores

Indicadores principais:

* saldo atual
* total de receitas
* total de despesas
* variação mensal
* progresso das metas

## Diretriz de implementação

Consultas de dashboard devem ser otimizadas para leitura frequente.

Onde necessário:

* usar índices adequados
* usar agregações bem definidas
* evitar consultas repetitivas em série

---

# 10. Metas Financeiras

## Modelo

Uma meta financeira representa um objetivo com:

* nome
* valor-alvo
* prazo
* valor já acumulado
* status
* progresso

## Cálculos

O sistema deve calcular:

* quanto falta
* percentual concluído
* economia mensal necessária
* previsão de conclusão

## Acompanhamento

Metas devem ser acompanhadas ao longo do tempo com atualização de progresso baseada em entradas financeiras ou aportes definidos pelo usuário.

## Progresso

O progresso deve ser exibido de forma compreensível, com atualização consistente entre frontend e backend.

## Notificações

Metas podem gerar notificações quando:

* estiverem atrasadas
* estiverem próximas do prazo
* houver risco de não atingir o objetivo

---

# 11. Simulador Financeiro

## Arquitetura dos cálculos

O simulador deve possuir módulos independentes por tipo de investimento.

Cada investimento deve ter sua própria lógica isolada, por exemplo:

* CDI
* Tesouro Selic
* LCI
* LCA

## Regras de implementação

1. Cálculos devem ser puros.
2. Cálculos não devem depender da interface.
3. Cada módulo deve ser testável isoladamente.
4. Parâmetros de entrada e saída devem ser explícitos.

## Separação de responsabilidades

* UI coleta dados do usuário.
* Hook orquestra a chamada.
* Service ou módulo de domínio executa o cálculo.
* Resultado é exibido em texto e gráfico.

## Saídas esperadas

O simulador deve exibir:

* rendimento bruto
* rendimento líquido
* imposto
* evolução mensal
* gráfico de desempenho

---

# 12. Notificações

## Eventos

Eventos que podem gerar notificação:

* conta próxima do vencimento
* meta atrasada
* orçamento excedido
* lembrete personalizado

## Geração

Notificações podem ser geradas por:

* eventos de domínio
* processamento agendado
* atualização de metas
* validação de regras financeiras

## Persistência

Notificações devem ser persistidas para permitir:

* histórico
* marcação como lida
* consulta posterior

## Exibição

O frontend deve apresentar notificações com:

* status lido/não lido
* prioridade
* data
* mensagem clara

---

# 13. Segurança

## Autenticação

* Access token em memória
* Refresh token em cookie HttpOnly
* Proteção contra brute force
* Logout com invalidação de sessão

## Autorização

* Policies por recurso
* Verificação de propriedade do dado
* Nenhum acesso indireto a dados de outros usuários

## Validação

* validação no frontend para UX
* validação obrigatória no backend para segurança
* schemas explícitos para requests

## Sanitização

* limpar entrada de texto
* normalizar dados importados
* evitar exposição de conteúdo sensível

## Rate limiting

* autenticação
* recuperação de senha
* importação
* endpoints sensíveis

## Proteção contra ataques

* SQL Injection por meio de queries parametrizadas e camada de repository
* XSS com escape e renderização segura
* CSRF nos fluxos com cookie
* Mass Assignment com preenchimento controlado

## Armazenamento seguro

* senhas com hashing forte
* tokens sensíveis nunca em LocalStorage
* logs sem dados financeiros sensíveis

---

# 14. Performance

## Cache

O cache deve ser usado de forma seletiva para:

* dados de leitura frequente
* consultas de dashboard
* metadados estáveis

## Lazy loading

Aplicar lazy loading para:

* rotas
* telas pesadas
* gráficos
* módulos pouco acessados

## Memoização

Memoização deve ser usada apenas onde houver ganho real:

* componentes pesados
* cálculos repetitivos
* listas derivadas

## Paginação

Toda listagem grande deve ser paginada.

Isso vale especialmente para:

* transações
* notificações
* importações

## Virtualização

Usar virtualização quando o volume de itens crescer para centenas ou milhares.

## Otimizações no banco

* índices adequados
* consultas específicas
* evitar `SELECT *`
* evitar N+1
* eager loading quando apropriado
* agregações pensadas para leitura

---

# 15. Testes

## Estratégia geral

Todo módulo crítico deve ser testado com foco em:

* regras de negócio
* validação
* segurança
* integração entre camadas
* regressão de cálculos

## Módulos com testes obrigatórios

* autenticação
* recuperação de senha
* importação CSV
* categorização
* transações
* metas
* simulador financeiro
* notificações
* dashboard agregado

## Organização dos testes

### Frontend

* testes de hook
* testes de componente quando houver lógica de interação relevante
* testes de validação de schema

### Backend

* testes de service
* testes de repository quando o comportamento for relevante
* testes de request
* testes de policy
* testes de integração da API

### Regras puras

* testes unitários para cálculos financeiros
* testes de categorização por palavra-chave e prioridade

## Cobertura

Cobertura mínima alvo:

* 80%

Mas a prioridade real deve ser qualidade sobre número absoluto.

---

# 16. Deploy

## Frontend

O frontend será implantado na Vercel.

Responsabilidades:

* build da aplicação
* publicação da interface
* configuração de variáveis de ambiente públicas

## Backend

O backend será implantado na Railway.

Responsabilidades:

* execução da API Laravel
* filas, jobs e agendamentos quando aplicável
* integração com banco e serviços externos

## Banco

O banco PostgreSQL deve estar em ambiente gerenciado, com:

* backup
* migração controlada
* variáveis de conexão protegidas

## Variáveis de ambiente

Devem existir variáveis separadas para:

* frontend
* backend
* banco
* chaves de autenticação
* serviços de e-mail
* observabilidade

## CI/CD

Pipeline mínimo:

1. instalar dependências
2. rodar lint
3. rodar testes
4. validar build
5. publicar apenas se tudo passar

---

# 17. Escalabilidade

## Open Finance

A arquitetura deve permitir adicionar integração bancária futura sem quebrar o modelo atual.

Recomendação:

* tratar fontes externas como “conectores”
* manter importação manual como fluxo independente

## Aplicativo mobile

Para suportar mobile no futuro:

* manter API desacoplada da interface
* preservar contratos consistentes
* evitar dependência de comportamento específico do navegador

## OCR

OCR deve entrar como novo pipeline de ingestão.

A arquitetura atual já suporta isso se:

* a importação for modular
* a normalização for isolada
* o parser for substituível

## IA

IA futura deve ser adicionada como serviço opcional, sem alterar a categorização determinística do MVP.

Isso evita dependência prematura de modelos complexos.

## Novas calculadoras

O simulador deve aceitar novos módulos por investimento ou produto financeiro.

A regra é:

* um módulo por cálculo
* uma interface de entrada e saída consistente

## Múltiplos idiomas

A aplicação deve manter textos e mensagens externalizáveis.

Isso facilita i18n futuro sem reescrever a UI.

## Múltiplas moedas

O modelo deve preservar moeda por transação, quando necessário.

Isso evita perda de informação e permite expansão para cenários multimoeda.

---

# Considerações finais

A arquitetura oficial do Orbit prioriza:

* simplicidade suficiente para o MVP
* separação de responsabilidades
* testabilidade
* segurança
* escalabilidade progressiva

As decisões definitivas são:

* frontend feature-based
* backend em camadas
* autenticação controlada pelo backend Laravel
* refresh token em cookie HttpOnly
* categorização determinística no MVP
* importações separadas das transações
* auditoria de eventos relevantes

Este documento deve ser seguido como referência para toda implementação futura.
