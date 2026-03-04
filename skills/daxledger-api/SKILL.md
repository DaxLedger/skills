---
name: daxledger-api
description: >
  Use the DAX Ledger API to authenticate, list portfolios, retrieve portfolio KPIs,
  and list/filter transactions with pagination.
---

# DAX Ledger API

Base URL
https://app.daxledger.io

This skill allows an agent to:

- Authenticate with the DAX Ledger API
- List available portfolios
- Retrieve KPIs for a specific portfolio
- List and filter transactions (with pagination)

All authenticated endpoints require a Bearer JWT token.

---

# Environment Variables

The API credentials must be provided through the following environment variables:

| Variable | Description |
|--------|-------------|
| DAXLEDGER_API_KEY | API key used to authenticate with the DAX Ledger API |
| DAXLEDGER_API_SECRET | API secret used together with the API key |

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
| List transactions (paged) | GET /api/portfolio/{portfolioId}/transactions?page=1&pageSize=20 | references/apis.md |
| Filter transactions by hash | GET /api/portfolio/{portfolioId}/transactions?filter=<BASE64> | references/apis.md |

---

# Recommended Workflow

Follow this sequence when interacting with the API.

### 1 Authenticate

POST /api/auth/external_api

Body:
```
{
  "APIKey": "{{DAXLEDGER_API_KEY}}",
  "APISecret": "{{DAXLEDGER_API_SECRET}}"
}
```
Store the returned token.

---

### 2 List portfolios

GET /api/portfolios

Select the portfolioId from the response.

---

### 3 Retrieve portfolio KPIs

GET /api/portfolio/{portfolioId}/kpis/portfolio

---

### 4 List / filter transactions

GET /api/portfolio/{portfolioId}/transactions?page=1&pageSize=20

Optionally add `filter=<BASE64>` (see Transactions section below).

---

# Transactions (List + Filters + Pagination)

## Endpoint
GET /api/portfolio/{portfolioId}/transactions

## Query parameters
- page (integer): page number, starts at 1
- pageSize (integer): number of items per page (default: 20)
- filter (string): Base64-encoded JSON filter (see below)

## Filters available
- Transaction Hash

### Filter format
Filters are provided as a JSON string, then Base64-encoded (using `btoa` or equivalent),
and sent in the `filter` query parameter.

Example (Transaction Hash contains "123456789"):

Raw filter JSON string:
{"transactionHash":{"operator":"contains","value":"123456789"}}

Then send:
GET /api/portfolio/{portfolioId}/transactions?filter=<BASE64>&page=1&pageSize=20

### Encoding examples

Browser:
btoa('{"transactionHash":{"operator":"contains","value":"123456789"}}')

Node.js:
Buffer.from('{"transactionHash":{"operator":"contains","value":"123456789"}}', 'utf8').toString('base64')

## Pagination iteration rule
- Start with page=1
- Keep requesting the next page while the number of collected items is still lower than `total`.
  - If collectedItems.length < total, increment page and fetch again.
  - Stop once collectedItems.length >= total.

Important: Some responses may nest the list as `response.items.items`.
Robust parsing should support:
- items = response.items (if array)
- items = response.items.items (if nested)
- total = response.total OR response.items.total

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
