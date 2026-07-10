# API.md

# API Oficial do Orbit

Este documento descreve os endpoints oficiais da API do Orbit.

Ele cobre:

* todos os endpoints do MVP;
* request;
* response;
* códigos HTTP;
* autenticação;
* paginação;
* filtros;
* exemplos.

Se houver conflito entre este documento e a arquitetura oficial, a arquitetura prevalece.

---

# 1. Convenções Gerais

## Base URL

```text
/api
```

## Formato de dados

* `Content-Type: application/json`
* respostas em JSON
* datas em ISO 8601
* valores monetários em decimal com precisão apropriada

## Padrão de resposta

### Sucesso

```json
{
  "data": {},
  "message": "Success"
}
```

### Lista paginada

```json
{
  "data": [],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 120,
    "last_page": 8
  }
}
```

### Erro

```json
{
  "message": "Validation failed",
  "errors": {
    "field": ["The field is required."]
  }
}
```

## Autenticação

### Mecanismo oficial

* access token em memória no frontend
* refresh token em cookie `HttpOnly`
* backend Laravel responsável por emissão e renovação

### Observação importante

A API segue um modelo compatível com o uso de access token em memória no frontend. O refresh token nunca deve ser exposto ao JavaScript.

### Como enviar credenciais

Endpoints protegidos devem receber:

```http
Authorization: Bearer <access_token>
```

Quando o access token expirar, o frontend deve chamar o endpoint de refresh.

## Códigos HTTP

* `200 OK` - sucesso em leitura ou mutação
* `201 Created` - recurso criado
* `204 No Content` - sucesso sem payload
* `400 Bad Request` - requisição inválida
* `401 Unauthorized` - não autenticado ou token inválido
* `403 Forbidden` - sem permissão
* `404 Not Found` - recurso inexistente
* `409 Conflict` - conflito de regra ou duplicidade
* `422 Unprocessable Entity` - validação falhou
* `429 Too Many Requests` - rate limit excedido
* `500 Internal Server Error` - erro inesperado

## Paginação

### Padrão

Paginação via query params:

* `page`
* `per_page`

### Exemplo

```text
GET /api/transactions?page=1&per_page=15
```

### Resposta

```json
{
  "data": [],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 0,
    "last_page": 0
  }
}
```

## Filtros gerais

Os endpoints de listagem podem aceitar:

* `search`
* `start_date`
* `end_date`
* `account_id`
* `category_id`
* `status`
* `type`
* `sort_by`
* `sort_direction`

---

# 2. Autenticação

## POST /auth/register

Cria uma nova conta de usuário.

### Autenticação

Não requer autenticação.

### Request

Campos obrigatórios:

* `name`
* `email`
* `password`
* `password_confirmation`

Regras de validação:

* `name` deve ser texto não vazio
* `email` deve ser válido e único
* `password` deve respeitar a política de senha definida pelo backend
* `password_confirmation` deve ser igual a `password`

```json
{
  "name": "João Silva",
  "email": "joao@example.com",
  "password": "SenhaForte@123",
  "password_confirmation": "SenhaForte@123"
}
```

### Response 201

```json
{
  "data": {
    "user": {
      "id": 1,
      "name": "João Silva",
      "email": "joao@example.com"
    },
    "access_token": "eyJ..."
  },
  "message": "User registered successfully"
}
```

### Códigos HTTP

* `201 Created`
* `422 Unprocessable Entity`
* `409 Conflict`

---

## POST /auth/login

Autentica o usuário e inicia a sessão.

### Autenticação

Não requer autenticação.

### Request

Campos obrigatórios:

* `email`
* `password`

Regras de validação:

* `email` deve ser um endereço válido
* `password` deve ser uma string não vazia

```json
{
  "email": "joao@example.com",
  "password": "SenhaForte@123"
}
```

### Response 200

```json
{
  "data": {
    "user": {
      "id": 1,
      "name": "João Silva",
      "email": "joao@example.com"
    },
    "access_token": "eyJ..."
  },
  "message": "Login successful"
}
```

### Códigos HTTP

* `200 OK`
* `401 Unauthorized`
* `422 Unprocessable Entity`
* `429 Too Many Requests`

---

## POST /auth/refresh

Renova o access token usando o refresh token armazenado em cookie HttpOnly.

### Autenticação

Requer cookie refresh válido.

### Comportamento esperado

* valida o refresh token recebido automaticamente pelo navegador
* emite um novo access token
* renova o refresh token quando a política de sessão exigir
* mantém o refresh token fora do alcance do JavaScript

### Request

Sem body obrigatório.

### Response 200

```json
{
  "data": {
    "access_token": "eyJ..."
  },
  "message": "Token refreshed successfully"
}
```

### Códigos HTTP

* `200 OK`
* `401 Unauthorized`
* `403 Forbidden`

---

## POST /auth/logout

Encerra a sessão do usuário.

### Autenticação

Requer access token válido.

### Comportamento esperado

* invalida a sessão ativa no backend
* remove o cookie `HttpOnly` associado ao refresh token, quando existir
* encerra a sessão sem expor dados adicionais ao frontend

### Request

Sem body obrigatório.

### Response 204

Sem payload.

### Códigos HTTP

* `204 No Content`
* `401 Unauthorized`

---

## GET /auth/me

Retorna o usuário autenticado.

### Autenticação

Requer access token válido.

### Response 200

```json
{
  "data": {
    "id": 1,
    "name": "João Silva",
    "email": "joao@example.com"
  }
}
```

### Códigos HTTP

* `200 OK`
* `401 Unauthorized`

---

## POST /auth/forgot-password

Solicita recuperação de senha.

### Autenticação

Não requer autenticação.

### Request

```json
{
  "email": "joao@example.com"
}
```

### Response 200

```json
{
  "message": "If the email exists, a recovery link was sent"
}
```

### Códigos HTTP

* `200 OK`
* `422 Unprocessable Entity`
* `429 Too Many Requests`

---

## POST /auth/reset-password

Redefine a senha usando token de recuperação.

### Autenticação

Não requer autenticação.

### Request

```json
{
  "token": "reset-token",
  "email": "joao@example.com",
  "password": "NovaSenha@123",
  "password_confirmation": "NovaSenha@123"
}
```

### Response 200

```json
{
  "message": "Password reset successfully"
}
```

### Códigos HTTP

* `200 OK`
* `422 Unprocessable Entity`
* `400 Bad Request`

---

# 3. Usuário

## GET /user

Retorna os dados do usuário autenticado.

### Autenticação

Requer access token válido.

### Response 200

```json
{
  "data": {
    "id": 1,
    "name": "João Silva",
    "email": "joao@example.com",
    "status": "active"
  }
}
```

---

## PATCH /user/profile

Atualiza dados básicos do perfil.

### Autenticação

Requer access token válido.

### Request

```json
{
  "name": "João S. Silva"
}
```

### Response 200

```json
{
  "data": {
    "id": 1,
    "name": "João S. Silva",
    "email": "joao@example.com"
  },
  "message": "Profile updated successfully"
}
```

---

# 4. Contas

## GET /accounts

Lista as contas do usuário autenticado.

### Autenticação

Requer access token válido.

### Query params

* `page`
* `per_page`
* `search`
* `status`
* `account_type`

### Response 200

```json
{
  "data": [
    {
      "id": 1,
      "name": "Conta Principal",
      "institution_name": "Banco X",
      "account_type": "checking",
      "currency_code": "BRL",
      "initial_balance": "1000.00",
      "current_balance": "1500.00",
      "status": "active"
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 1,
    "last_page": 1
  }
}
```

---

## POST /accounts

Cria uma nova conta.

### Autenticação

Requer access token válido.

### Request

```json
{
  "name": "Conta Principal",
  "institution_name": "Banco X",
  "account_type": "checking",
  "currency_code": "BRL",
  "initial_balance": 1000.0
}
```

### Response 201

```json
{
  "data": {
    "id": 1,
    "name": "Conta Principal",
    "institution_name": "Banco X",
    "account_type": "checking",
    "currency_code": "BRL",
    "initial_balance": "1000.00",
    "current_balance": "1000.00",
    "status": "active"
  },
  "message": "Account created successfully"
}
```

---

## GET /accounts/{account}

Exibe uma conta específica.

### Autenticação

Requer access token válido.

### Response 200

```json
{
  "data": {
    "id": 1,
    "name": "Conta Principal",
    "institution_name": "Banco X"
  }
}
```

---

## PATCH /accounts/{account}

Atualiza uma conta.

### Autenticação

Requer access token válido.

### Request

```json
{
  "name": "Conta Corrente Principal",
  "status": "active"
}
```

### Response 200

```json
{
  "data": {
    "id": 1,
    "name": "Conta Corrente Principal",
    "status": "active"
  },
  "message": "Account updated successfully"
}
```

---

## DELETE /accounts/{account}

Remove uma conta logicamente, quando aplicável.

### Autenticação

Requer access token válido.

### Response 204

Sem payload.

---

# 5. Categorias

## GET /categories

Lista categorias globais e personalizadas do usuário.

### Autenticação

Requer access token válido.

### Query params

* `page`
* `per_page`
* `search`
* `scope`
* `status`

### Response 200

```json
{
  "data": [
    {
      "id": 1,
      "name": "Transporte",
      "slug": "transporte",
      "scope": "global",
      "color": "#2563eb",
      "icon": "car"
    }
  ]
}
```

---

## POST /categories

Cria uma categoria personalizada.

### Autenticação

Requer access token válido.

### Request

```json
{
  "name": "Padaria",
  "color": "#f59e0b",
  "icon": "bread",
  "parent_id": null
}
```

### Response 201

```json
{
  "data": {
    "id": 10,
    "name": "Padaria",
    "slug": "padaria",
    "scope": "user"
  },
  "message": "Category created successfully"
}
```

---

## PATCH /categories/{category}

Atualiza uma categoria.

### Autenticação

Requer access token válido.

### Request

```json
{
  "name": "Mercado",
  "color": "#10b981"
}
```

### Response 200

```json
{
  "data": {
    "id": 10,
    "name": "Mercado"
  },
  "message": "Category updated successfully"
}
```

---

## DELETE /categories/{category}

Remove ou desativa uma categoria.

### Autenticação

Requer access token válido.

### Response 204

Sem payload.

---

# 6. Transações

## GET /transactions

Lista transações do usuário autenticado.

### Autenticação

Requer access token válido.

### Query params

* `page`
* `per_page`
* `search`
* `start_date`
* `end_date`
* `account_id`
* `category_id`
* `type`
* `status`
* `source_type`
* `sort_by`
* `sort_direction`

### Response 200

```json
{
  "data": [
    {
      "id": 1,
      "description": "Uber",
      "amount": "-32.90",
      "transaction_date": "2026-06-26",
      "transaction_type": "expense",
      "category": {
        "id": 3,
        "name": "Transporte"
      },
      "account": {
        "id": 1,
        "name": "Conta Principal"
      },
      "status": "posted"
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 1,
    "last_page": 1
  }
}
```

---

## POST /transactions

Cria uma transação manual.

### Autenticação

Requer access token válido.

### Request

```json
{
  "description": "Uber",
  "amount": -32.9,
  "transaction_date": "2026-06-26",
  "transaction_type": "expense",
  "account_id": 1,
  "category_id": 3,
  "notes": "Volta para casa"
}
```

### Response 201

```json
{
  "data": {
    "id": 1,
    "description": "Uber",
    "amount": "-32.90",
    "transaction_date": "2026-06-26",
    "transaction_type": "expense"
  },
  "message": "Transaction created successfully"
}
```

---

## GET /transactions/{transaction}

Exibe uma transação específica.

### Autenticação

Requer access token válido.

### Response 200

```json
{
  "data": {
    "id": 1,
    "description": "Uber",
    "amount": "-32.90",
    "transaction_date": "2026-06-26"
  }
}
```

---

## PATCH /transactions/{transaction}

Atualiza uma transação.

### Autenticação

Requer access token válido.

### Request

```json
{
  "description": "Uber Black",
  "category_id": 3,
  "notes": "Atualizado manualmente"
}
```

### Response 200

```json
{
  "data": {
    "id": 1,
    "description": "Uber Black"
  },
  "message": "Transaction updated successfully"
}
```

---

## DELETE /transactions/{transaction}

Remove logicamente uma transação.

### Autenticação

Requer access token válido.

### Response 204

Sem payload.

---

## PATCH /transactions/{transaction}/category

Atualiza a categoria de uma transação e pode gerar regra personalizada.

### Autenticação

Requer access token válido.

### Request

```json
{
  "category_id": 4
}
```

### Response 200

```json
{
  "data": {
    "transaction_id": 1,
    "category_id": 4
  },
  "message": "Transaction category updated successfully"
}
```

---

# 7. Importação

## POST /imports/csv

Importa um arquivo CSV de transações.

### Autenticação

Requer access token válido.

### Request

`multipart/form-data`

Campos:

* `file`
* `account_id`
* `source`

### Response 201

```json
{
  "data": {
    "import_batch_id": 1,
    "status": "processed",
    "total_rows": 120,
    "imported_rows": 118,
    "rejected_rows": 2
  },
  "message": "CSV imported successfully"
}
```

### Códigos HTTP

* `201 Created`
* `422 Unprocessable Entity`
* `415 Unsupported Media Type`
* `409 Conflict`

---

## GET /imports

Lista lotes de importação.

### Autenticação

Requer access token válido.

### Query params

* `page`
* `per_page`
* `status`
* `source`
* `start_date`
* `end_date`

### Response 200

```json
{
  "data": [
    {
      "id": 1,
      "file_name": "extrato.csv",
      "status": "processed",
      "total_rows": 120,
      "imported_rows": 118,
      "rejected_rows": 2
    }
  ]
}
```

---

## GET /imports/{importBatch}

Exibe detalhes de um lote de importação.

### Autenticação

Requer access token válido.

### Response 200

```json
{
  "data": {
    "id": 1,
    "file_name": "extrato.csv",
    "status": "processed",
    "items": []
  }
}
```

---

## GET /imports/{importBatch}/items

Lista os itens de um lote de importação.

### Autenticação

Requer access token válido.

### Query params

* `page`
* `per_page`
* `status`

### Response 200

```json
{
  "data": [
    {
      "id": 1,
      "line_number": 10,
      "raw_description": "Uber",
      "normalized_description": "Uber",
      "status": "imported"
    }
  ]
}
```

---

# 8. Metas

## GET /goals

Lista metas do usuário autenticado.

### Autenticação

Requer access token válido.

### Query params

* `page`
* `per_page`
* `status`
* `start_date`
* `end_date`

### Response 200

```json
{
  "data": [
    {
      "id": 1,
      "name": "Comprar notebook",
      "target_amount": "7000.00",
      "saved_amount": "1500.00",
      "progress_percentage": 21.43,
      "estimated_completion_date": "2027-04-01",
      "status": "active"
    }
  ]
}
```

---

## POST /goals

Cria uma meta financeira.

### Autenticação

Requer access token válido.

### Request

```json
{
  "name": "Comprar notebook",
  "target_amount": 7000,
  "target_date": "2027-04-01",
  "description": "Meta para compra de equipamento"
}
```

### Response 201

```json
{
  "data": {
    "id": 1,
    "name": "Comprar notebook",
    "target_amount": "7000.00",
    "saved_amount": "0.00",
    "progress_percentage": 0
  },
  "message": "Goal created successfully"
}
```

---

## GET /goals/{goal}

Exibe uma meta.

### Autenticação

Requer access token válido.

### Response 200

```json
{
  "data": {
    "id": 1,
    "name": "Comprar notebook",
    "target_amount": "7000.00"
  }
}
```

---

## PATCH /goals/{goal}

Atualiza uma meta.

### Autenticação

Requer access token válido.

### Request

```json
{
  "name": "Comprar notebook gamer",
  "target_date": "2027-06-01"
}
```

### Response 200

```json
{
  "data": {
    "id": 1,
    "name": "Comprar notebook gamer"
  },
  "message": "Goal updated successfully"
}
```

---

## PATCH /goals/{goal}/progress

Atualiza o progresso da meta.

### Autenticação

Requer access token válido.

### Request

```json
{
  "saved_amount": 2500
}
```

### Response 200

```json
{
  "data": {
    "id": 1,
    "saved_amount": "2500.00",
    "progress_percentage": 35.71
  },
  "message": "Goal progress updated successfully"
}
```

---

## DELETE /goals/{goal}

Remove logicamente uma meta.

### Autenticação

Requer access token válido.

### Response 204

Sem payload.

---

# 9. Simulador de investimentos

## POST /investments/simulations

Executa simulação financeira.

### Autenticação

Requer access token válido.

### Request

```json
{
  "product": "cdi",
  "initial_amount": 1000,
  "monthly_contribution": 200,
  "months": 12,
  "annual_rate": 10.5
}
```

### Response 200

```json
{
  "data": {
    "product": "cdi",
    "gross_return": "220.00",
    "net_return": "178.00",
    "tax": "42.00",
    "timeline": []
  }
}
```

### Códigos HTTP

* `200 OK`
* `422 Unprocessable Entity`
* `400 Bad Request`

---

# 10. Notificações

## GET /notifications

Lista notificações do usuário.

### Autenticação

Requer access token válido.

### Query params

* `page`
* `per_page`
* `read`
* `type`
* `severity`

### Response 200

```json
{
  "data": [
    {
      "id": 1,
      "type": "goal_overdue",
      "title": "Meta atrasada",
      "message": "Sua meta está atrasada em 10 dias.",
      "severity": "high",
      "read_at": null
    }
  ]
}
```

---

## PATCH /notifications/{notification}/read

Marca uma notificação como lida.

### Autenticação

Requer access token válido.

### Response 200

```json
{
  "data": {
    "id": 1,
    "read_at": "2026-06-26T12:00:00Z"
  },
  "message": "Notification marked as read"
}
```

---

## PATCH /notifications/read-all

Marca todas as notificações como lidas.

### Autenticação

Requer access token válido.

### Response 200

```json
{
  "message": "All notifications marked as read"
}
```

---

# 11. Dashboard

## GET /dashboard/summary

Retorna os indicadores principais do usuário.

### Autenticação

Requer access token válido.

### Query params

* `start_date`
* `end_date`
* `account_id`
* `category_id`

### Response 200

```json
{
  "data": {
    "balance": "1500.00",
    "income": "5000.00",
    "expenses": "3500.00",
    "monthly_spent": "1200.00",
    "monthly_growth": 8.2
  }
}
```

---

## GET /dashboard/charts

Retorna dados consolidados para os gráficos.

### Autenticação

Requer access token válido.

### Query params

* `start_date`
* `end_date`
* `account_id`
* `category_id`

### Response 200

```json
{
  "data": {
    "income_vs_expenses": [],
    "category_spending": [],
    "monthly_evolution": [],
    "cash_flow": [],
    "net_worth": []
  }
}
```

---

# 12. Endpoints de suporte

## GET /health

Verifica disponibilidade da API.

### Autenticação

Não requer autenticação.

### Response 200

```json
{
  "status": "ok"
}
```

---

# 13. Regras de autorização

Todos os endpoints protegidos devem validar:

* o usuário está autenticado;
* o recurso pertence ao usuário;
* o recurso pode ser acessado no estado atual;
* o usuário pode executar a ação solicitada.

Se a validação falhar:

* `401 Unauthorized` para sessão ausente ou inválida;
* `403 Forbidden` para falta de permissão;
* `404 Not Found` pode ser usado para esconder a existência do recurso, quando apropriado por segurança.

---

# 14. Estratégia de paginação e filtros

## Paginação

As listagens principais usam paginação padrão com:

* `page`
* `per_page`

## Filtros aplicáveis por recurso

### Transactions

* `search`
* `start_date`
* `end_date`
* `account_id`
* `category_id`
* `type`
* `status`
* `source_type`

### Imports

* `status`
* `source`
* `start_date`
* `end_date`

### Goals

* `status`
* `start_date`
* `end_date`

### Notifications

* `read`
* `type`
* `severity`

### Dashboard

* `start_date`
* `end_date`
* `account_id`
* `category_id`

---

# 15. Exemplos rápidos

## Login

```http
POST /api/auth/login
```

```json
{
  "email": "joao@example.com",
  "password": "SenhaForte@123"
}
```

## Criar transação

```http
POST /api/transactions
```

```json
{
  "description": "iFood",
  "amount": -58.9,
  "transaction_date": "2026-06-26",
  "transaction_type": "expense",
  "account_id": 1,
  "category_id": 2
}
```

## Importar CSV

```http
POST /api/imports/csv
```

`multipart/form-data`

---

# 16. Observações finais

1. Endpoints não descritos explicitamente neste documento devem ser considerados fora do MVP.
2. Toda resposta deve ser previsível e padronizada.
3. Toda alteração contratual futura deve atualizar este documento antes da implementação.
