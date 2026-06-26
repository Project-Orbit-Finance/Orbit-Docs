# ROADMAP.md

## Visão geral do projeto

Orbit é uma plataforma web de finanças pessoais focada em importação de transações, categorização automática, acompanhamento financeiro, metas e visualização de indicadores. A arquitetura oficial prioriza simplicidade, segurança, testabilidade e evolução incremental.

## Objetivo do MVP

Entregar uma base funcional e confiável para:

* autenticação segura
* cadastro e gerenciamento de transações
* importação de CSV
* categorização automática por regras
* dashboard financeiro com gráficos e indicadores
* metas financeiras
* simulador de investimentos
* notificações básicas

## Milestones em ordem cronológica

1. [MILESTONE-01-Foundation](./MILESTONE-01-Foundation.md)
2. [MILESTONE-02-Authentication](./MILESTONE-02-Authentication.md)
3. [MILESTONE-03-Dashboard](./MILESTONE-03-Dashboard.md)
4. [MILESTONE-04-Transactions](./MILESTONE-04-Transactions.md)
5. [MILESTONE-05-Import](./MILESTONE-05-Import.md)
6. [MILESTONE-06-Goals](./MILESTONE-06-Goals.md)
7. [MILESTONE-07-Investments](./MILESTONE-07-Investments.md)
8. [MILESTONE-08-Notifications](./MILESTONE-08-Notifications.md)
9. [MILESTONE-09-Testing](./MILESTONE-09-Testing.md)
10. [MILESTONE-10-Deployment](./MILESTONE-10-Deployment.md)
11. [MILESTONE-11-Future](./MILESTONE-11-Future.md)

## Dependências entre milestones

* Foundation é pré-requisito para todos os demais milestones.
* Authentication depende de Foundation.
* Dashboard depende de Foundation e Authentication.
* Transactions depende de Foundation e Authentication.
* Import depende de Foundation, Authentication e Transactions.
* Goals depende de Foundation, Authentication e Transactions.
* Investments depende de Foundation.
* Notifications depende de Foundation, Authentication, Transactions e Goals.
* Testing depende de todos os milestones centrais para consolidar cobertura.
* Deployment depende de Foundation e dos fluxos principais estarem consolidados.
* Future depende da entrega do MVP e das decisões técnicas estabilizadas.

## Estimativa de complexidade

* MILESTONE-01-Foundation: Alta
* MILESTONE-02-Authentication: Alta
* MILESTONE-03-Dashboard: Alta
* MILESTONE-04-Transactions: Alta
* MILESTONE-05-Import: Alta
* MILESTONE-06-Goals: Média
* MILESTONE-07-Investments: Média
* MILESTONE-08-Notifications: Média
* MILESTONE-09-Testing: Alta
* MILESTONE-10-Deployment: Média
* MILESTONE-11-Future: Baixa

## Riscos técnicos

* inconsistência entre cálculos do frontend e backend
* modelagem insuficiente para transações importadas e auditabilidade
* importação CSV com duplicidade e parsing inconsistente
* autenticação com refresh token exigindo proteção CSRF e rotação correta
* consultas de dashboard com custo alto sem índices e agregações adequadas
* crescimento do domínio financeiro sem separação suficiente entre regras e infraestrutura

## Funcionalidades fora do MVP

* Open Finance
* sincronização bancária automática
* IA para sugestões financeiras
* OCR de comprovantes
* aplicativo mobile
* compartilhamento familiar
* múltiplas moedas completas
* exportação para Excel/PDF
* artigos próprios de educação financeira

## Critério geral de conclusão do roadmap

O roadmap será considerado concluído quando:

* o MVP estiver implementado e testado
* o deploy estiver definido e documentado
* a documentação técnica estiver consistente com a implementação
* as funcionalidades fora do MVP estiverem registradas como futuras
