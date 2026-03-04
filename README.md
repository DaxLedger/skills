# DAX Ledger Skills

Agent Skills for integrating **DAX Ledger portfolio APIs** into AI-powered applications.

These skills allow agents to authenticate with the DAX Ledger API, list portfolios, retrieve portfolio KPIs,
and list/filter portfolio transactions.

---

# Skills

## skills/daxledger-api

Query portfolio information and analytics from the DAX Ledger platform.

Capabilities include:

- Authenticating with the DAX Ledger API
- Listing available portfolios
- Retrieving portfolio KPIs
- Listing and filtering portfolio transactions (paged)

Auth: APIKey + APISecret exchanged for a JWT token  
Base URL: https://app.daxledger.io

Entry point:

skills/daxledger-api/SKILL.md

---

# Which skill should I use?

| Scenario                                       | Skill         |
| ---------------------------------------------- | ------------- |
| I need to authenticate with the DAX Ledger API | daxledger-api |
| I need to list available portfolios            | daxledger-api |
| I need to retrieve KPIs for a portfolio        | daxledger-api |
| I need to list transactions for a portfolio    | daxledger-api |
| I need to filter transactions by hash          | daxledger-api |

---

# Environment Variables

The skill requires the following environment variables:

| Variable             | Description                                  |
| -------------------- | -------------------------------------------- |
| DAXLEDGER_API_KEY    | API key used to authenticate with DAX Ledger |
| DAXLEDGER_API_SECRET | API secret used together with the API key    |

These credentials are exchanged for a JWT token via the authentication endpoint.

---

# Installation

If the skill is published in a skills registry:

npx skills add daxledger/skills --yes

Or manually copy the folder into your project:

skills/
└─ daxledger-api/
├─ SKILL.md
└─ references/
└─ apis.md

---

# API Workflow

Typical workflow when using this skill:

### 1 Authenticate

POST /api/auth/external_api

Body:

{
"APIKey": "{{DAXLEDGER_API_KEY}}",
"APISecret": "{{DAXLEDGER_API_SECRET}}"
}

Response:

{
"token": "jwt_token_here"
}

---

### 2 List portfolios

GET /api/portfolios

Optional query:

GET /api/portfolios?show=owned

---

### 3 Retrieve portfolio KPIs

GET /api/portfolio/{portfolioId}/kpis/portfolio

---

### 4 List / filter transactions

GET /api/portfolio/{portfolioId}/transactions?page=1&pageSize=20

Add filters using the `filter` query param (Base64-encoded JSON string).

---

# Transactions (List + Filters)

Endpoint:

GET /api/portfolio/{portfolioId}/transactions

## Pagination

- `page` starts at **1**
- `pageSize` default is **20**
- If the number of collected items is still lower than `total`, request the next page.

Practical rule:

- fetch page 1
- while `items.length < total` → increment `page` and fetch again

## Filters

Filters are sent via query param `filter` and must be a **Base64-encoded JSON string**.

### Available filter

- Transaction Hash

### Filter format (transaction hash contains)

Raw filter JSON string:

{"transactionHash":{"operator":"contains","value":"123456789"}}

Encode it (btoa/base64) and send it as:

GET /api/portfolio/{portfolioId}/transactions?filter=<BASE64>&page=1&pageSize=20

Browser:

btoa('{"transactionHash":{"operator":"contains","value":"123456789"}}')

Node.js:

Buffer.from('{"transactionHash":{"operator":"contains","value":"123456789"}}', 'utf8').toString('base64')

---

# Specification

This skill follows the Agent Skills specification used for LLM tool integration.

---

# API Reference

Detailed endpoint documentation is available in:

skills/daxledger-api/references/apis.md

---

# License

MIT
