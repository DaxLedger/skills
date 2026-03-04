---
name: daxledger-api
description: >
  Use the DAX Ledger API to authenticate, list portfolios, and retrieve
  KPIs for a portfolio.
---

# DAX Ledger API

Base URL
https://app.daxledger.io

This skill allows an agent to:

- Authenticate with the DAX Ledger API
- List available portfolios
- Retrieve KPIs for a specific portfolio

All authenticated endpoints require a Bearer JWT token.

---

# Environment Variables

The API credentials must be provided through the following environment variables:

| Variable | Description |
|--------|-------------|
| DAXLEDGER_API_KEY | API key used to authenticate with the DAX Ledger API |
| DAXLEDGER_API_SECRET | API secret used together with the API key |

These variables are used when calling the authentication endpoint.

---

# Authentication

## Endpoint
POST /api/auth/external_api

Exchange an APIKey + APISecret for a JWT token.

### Request

{
  "APIKey": "{{DAXLEDGER_API_KEY}}",
  "APISecret": "{{DAXLEDGER_API_SECRET}}"
}

### Response

{
  "token": "your_access_token_here"
}

### Usage

All subsequent requests must include:

Authorization: Bearer {{token}}

---

# Pick Your Endpoint

Wrong choice wastes a call. Match the task:

| You need | Hit this | Ref |
|--------|--------|-----|
| Authenticate API access | POST /api/auth/external_api | references/apis.md |
| List portfolios | GET /api/portfolios | references/apis.md |
| List owned portfolios only | GET /api/portfolios?show=owned | references/apis.md |
| List shared portfolios | GET /api/portfolios?show=shared | references/apis.md |
| Get portfolio KPIs | GET /api/portfolio/{portfolioId}/kpis/portfolio | references/apis.md |

---

# Recommended Workflow

Follow this sequence when interacting with the API.

### 1 Authenticate

POST /api/auth/external_api

Body:

{
  "APIKey": "{{DAXLEDGER_API_KEY}}",
  "APISecret": "{{DAXLEDGER_API_SECRET}}"
}

Store the returned token.

---

### 2 List portfolios

GET /api/portfolios

Select the portfolioId from the response.

---

### 3 Retrieve portfolio KPIs

GET /api/portfolio/{portfolioId}/kpis/portfolio

---

# Notes for Agents

- The portfolioId must come from the /api/portfolios response.
- Always authenticate before calling portfolio endpoints.
- If a request returns 401, obtain a new token.
- The KPI response may contain multiple KPI objects with fields:
  - kpiName
  - kpiType
  - kpiValue

Possible KPI types:

kpi
chart
information
barChart
table

---

# References

See detailed endpoint descriptions:

references/apis.md
