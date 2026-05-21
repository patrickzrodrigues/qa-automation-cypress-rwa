# Suíte de Testes E2E — Cypress Real World App

[![Cypress](https://img.shields.io/badge/Cypress-15.13.0-04C38E?logo=cypress&logoColor=white)](https://cypress.io)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript&logoColor=white)](https://typescriptlang.org)
[![Node.js](https://img.shields.io/badge/Node.js-20%2B-339933?logo=node.js&logoColor=white)](https://nodejs.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Suíte completa de testes End-to-End e de API desenvolvida como exercício prático de automação de testes QA, utilizando a **Cypress Real World App** como aplicação-alvo.

---

## Sobre o Projeto

Este repositório contém **90 casos de teste automatizados** cobrindo os principais fluxos de uma aplicação financeira full-stack — desde autenticação e transferências até gerenciamento de contas e notificações. O projeto foi desenvolvido com foco em boas práticas de QA: isolamento de estado entre testes, comandos reutilizáveis, interceptação de rede e suporte a múltiplos viewports.

## Aplicação Testada

A [Cypress Real World App](https://github.com/cypress-io/cypress-realworld-app) é uma aplicação financeira full-stack criada pela equipe do Cypress.io para simular cenários reais de produção. Ela inclui:

- Front-end em **React + TypeScript + Vite**
- Back-end em **Express + TypeScript**
- Banco de dados em **lowdb** (JSON)
- Autenticação via sessão com suporte a OAuth (Auth0, Okta, Cognito)

---

## Cobertura de Testes

### Testes de Interface (UI) — 39 casos em 7 suítes

| Suíte | Cenários Cobertos |
|---|---|
| `auth.spec.ts` | Registro, login, logout, "lembrar por 30 dias", redirecionamentos e validações de formulário |
| `new-transaction.spec.ts` | Pagamento, solicitação de dinheiro e validação de saldo insuficiente |
| `transaction-feeds.spec.ts` | Feed público, pessoal e de contatos; filtros por data e valor |
| `transaction-view.spec.ts` | Like, comentário, aceitar e recusar solicitação de pagamento |
| `bankaccounts.spec.ts` | Criar, listar e excluir contas bancárias |
| `notifications.spec.ts` | Notificações de like, comentário, pagamento recebido e solicitação |
| `user-settings.spec.ts` | Atualização de perfil e validações de campos do formulário |

### Testes de API — 51 casos em 9 suítes

| Suíte | Endpoints | Casos |
|---|---|:---:|
| `api-users.spec.ts` | `GET /users`, `GET /users/:id`, `PATCH /users/:id` | 13 |
| `api-transactions.spec.ts` | `GET /transactions`, `POST /transactions`, `PATCH /transactions/:id` | 10 |
| `api-bankaccounts.spec.ts` | `GET`, `POST`, `DELETE /bankaccounts` via GraphQL | 7 |
| `api-testdata.spec.ts` | `GET /testData/:entity` (8 entidades diferentes) | 8 |
| `api-notifications.spec.ts` | `GET /notifications`, `PATCH /notifications/:id` | 4 |
| `api-contacts.spec.ts` | `GET`, `POST`, `DELETE /contacts` | 4 |
| `api-comments.spec.ts` | `POST /comments`, `GET /comments` | 2 |
| `api-likes.spec.ts` | `POST /likes` | 2 |
| `api-banktransfers.spec.ts` | `GET /banktransfers` | 1 |

**Total: 90 casos de teste · 16 suítes · TypeScript**

---

## Técnicas Aplicadas

| Técnica | Descrição |
|---|---|
| `cy.intercept()` | Interceptação e validação de requisições HTTP em cada cenário |
| `cy.task("db:seed")` | Reset do banco de dados antes de cada teste para isolamento de estado |
| `cy.database()` | Acesso programático ao banco para setup dinâmico de dados |
| `cy.loginByXstate()` / `cy.loginByApi()` | Login programático sem interação com a UI, acelerando a execução |
| `cy.getBySel()` / `cy.getBySelLike()` | Seleção semântica via atributos `data-test`, desacoplada de CSS e estrutura DOM |
| Alias de requisições | Uso de `.as()` e `cy.wait()` para sincronização confiável com o back-end |
| Suporte mobile | Testes adaptados para viewport 375×667 via `isMobile()` |
| TypeScript | Tipagem forte nos contextos de teste usando modelos de domínio da aplicação |

---

## Estrutura do Projeto

```
cypress-realworld-app/
├── cypress/
│   ├── support/
│   │   ├── commands.ts        # Comandos customizados do Cypress
│   │   ├── e2e.ts             # Hooks globais (beforeEach, afterEach)
│   │   └── utils.ts           # Funções utilitárias (isMobile, etc.)
│   └── tests/
│       ├── api/               # 9 suítes de teste de API REST + GraphQL
│       │   ├── api-bankaccounts.spec.ts
│       │   ├── api-banktransfers.spec.ts
│       │   ├── api-comments.spec.ts
│       │   ├── api-contacts.spec.ts
│       │   ├── api-likes.spec.ts
│       │   ├── api-notifications.spec.ts
│       │   ├── api-testdata.spec.ts
│       │   ├── api-transactions.spec.ts
│       │   └── api-users.spec.ts
│       └── ui/                # 7 suítes de teste de interface
│           ├── auth.spec.ts
│           ├── bankaccounts.spec.ts
│           ├── new-transaction.spec.ts
│           ├── notifications.spec.ts
│           ├── transaction-feeds.spec.ts
│           ├── transaction-view.spec.ts
│           └── user-settings.spec.ts
├── backend/                   # API Express (TypeScript)
└── src/                       # React front-end (Vite)
```

---

## Como Executar

### Pré-requisitos

- Node.js 20+
- Yarn

### Instalação

```bash
# Clone o repositório com o submodule
git clone --recurse-submodules https://github.com/patrickzrodrigues/cypress-Test-cy.RWA-Exerc-cios.git

# Entre na pasta da aplicação
cd cypress-Test-cy.RWA-Exerc-cios/cypress-realworld-app

# Instale as dependências
yarn install

# Inicie a aplicação (front-end + back-end em paralelo)
yarn dev
```

### Rodando os Testes

```bash
# Modo interativo — abre o Cypress Test Runner
yarn cypress:open

# Modo headless — executa todos os testes no terminal
yarn cypress:run

# Apenas testes de API
yarn test:api

# Apenas testes de UI
yarn cypress:run --spec 'cypress/tests/ui/**/*.spec.ts'

# Modo mobile (viewport 375×667)
yarn cypress:run:mobile
```

---

## Habilidades Demonstradas

- Automação E2E com Cypress em aplicação full-stack de produção
- Separação clara entre testes de UI e de API no mesmo projeto
- Gerenciamento de estado de teste via seed de banco de dados
- Interceptação e asserção de requisições HTTP
- Tipagem TypeScript em contextos de teste
- Comandos customizados para reusabilidade e legibilidade
- Organização de suítes por módulo de negócio
- Testes responsivos com suporte a múltiplos viewports

---

Desenvolvido por **Patrick Zachett Rodrigues** · [GitHub](https://github.com/patrickzrodrigues)
