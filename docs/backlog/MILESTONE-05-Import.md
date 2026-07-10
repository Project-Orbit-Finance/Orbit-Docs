# MILESTONE-05-Import

## Objetivo

Implementar a importação de CSV como pipeline seguro, rastreável e idempotente.

## Dependências

* MILESTONE-01-Foundation
* MILESTONE-02-Authentication
* MILESTONE-04-Transactions

## Épico 1 — Upload e validação

### Descrição

Implementar o fluxo de envio e verificação do arquivo importado.

### Objetivo

Aceitar apenas arquivos CSV válidos e preparados para processamento.

### Dependências

Transações e autenticação.

### Feature 5.1 — Entrada de arquivo

**Objetivo:** organizar a recepção do CSV.  
**Descrição:** estruturar limites, tamanho e tipo aceito.  
**Critérios de aceitação:** contrato de upload fechado.

#### Tasks

* **ID:** F05.01.01
  * **Título:** Implementar contrato de upload CSV
  * **Descrição:** criar restrições de formato, tamanho e campos.
  * **Prioridade:** Alta
  * **Dependências:** F04.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Http/Requests/*`, `ARCHITECTURE.md`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** formato de upload definido.
  * **Definition of Done:** pipeline de entrada compreensível.

## Épico 2 — Parser e normalização

### Descrição

Transformar CSV bruto em itens de importação normalizados.

### Objetivo

Converter dados externos em estrutura interna segura.

### Dependências

Upload validado.

### Feature 5.2 — Parsing

**Objetivo:** organizar a leitura das linhas do CSV.  
**Descrição:** converter colunas em dados internos.  
**Critérios de aceitação:** parser descrito e isolado.

#### Tasks

* **ID:** F05.02.01
  * **Título:** Implementar mapeamento de colunas CSV
  * **Descrição:** criar como cada coluna será interpretada.
  * **Prioridade:** Alta
  * **Dependências:** F05.01.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Import/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** colunas esperadas mapeadas.
  * **Definition of Done:** contrato de parser pronto.

* **ID:** F05.02.02
  * **Título:** Implementar normalização de valores
  * **Descrição:** criar tratamento de datas, moedas, descrições e sinais.
  * **Prioridade:** Alta
  * **Dependências:** F05.02.01
  * **Arquivos que provavelmente serão modificados:** `app/Services/Import/*`, `app/Utils/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** regra de normalização clara.
  * **Definition of Done:** dados brutos convertíveis em estrutura interna.

## Épico 3 — Persistência e rastreabilidade

### Descrição

Registrar importações e associar itens às transações.

### Objetivo

Garantir histórico, auditoria e deduplicação.

### Dependências

Parser e normalização.

### Feature 5.3 — Lote de importação

**Objetivo:** implementar persistência do batch importado.  
**Descrição:** estruturar lote, itens e vínculo com transações.  
**Critérios de aceitação:** rastreabilidade definida.

#### Tasks

* **ID:** F05.03.01
  * **Título:** Implementar modelo de lote de importação
  * **Descrição:** criar o relacionamento entre lote e itens importados.
  * **Prioridade:** Alta
  * **Dependências:** F05.02.02
  * **Arquivos que provavelmente serão modificados:** `ARCHITECTURE.md`
  * **Documentação relacionada:** `ADR-001.md`, `ARCHITECTURE.md`
  * **Critérios de aceitação:** persistência separada das transações descrita.
  * **Definition of Done:** rastreabilidade do batch formalizada.

* **ID:** F05.03.02
  * **Título:** Implementar estratégia de deduplicação
  * **Descrição:** criar como evitar importação repetida.
  * **Prioridade:** Alta
  * **Dependências:** F05.03.01
  * **Arquivos que provavelmente serão modificados:** `app/Repositories/*`, `app/Services/Import/*`
  * **Documentação relacionada:** `ARCHITECTURE.md`
  * **Critérios de aceitação:** estratégia de idempotência descrita.
  * **Definition of Done:** risco de duplicidade tratado no desenho.
