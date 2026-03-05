
---
name: daxledger-api
description: >
  Use the DAX Ledger API to authenticate, list portfolios, retrieve portfolio KPIs,
  and list/filter transactions with pagination.
---

# DAX Ledger API

Base URL  
https://app.daxledger.io

---

# Environment Variables

| Variable | Description |
|--------|-------------|
| DAXLEDGER_API_KEY | API key used to authenticate |
| DAXLEDGER_API_SECRET | API secret used to authenticate |

---

# Authentication

POST /api/auth/external_api

Body:

{
  "APIKey": "{{DAXLEDGER_API_KEY}}",
  "APISecret": "{{DAXLEDGER_API_SECRET}}"
}

Response:

{
  "token": "your_access_token_here"
}

Header for authenticated calls:

Authorization: Bearer {{token}}

---

# Pick Your Endpoint

| You need | Endpoint | Ref |
|--------|--------|-----|
| Authenticate | POST /api/auth/external_api | references/apis.md |
| List portfolios | GET /api/portfolios | references/apis.md |
| Get KPIs | GET /api/portfolio/{portfolioId}/kpis/portfolio | references/apis.md |
| List transactions | GET /api/portfolio/{portfolioId}/transactions?page=1&pageSize=20 | references/apis.md |
| Filter transactions | GET /api/portfolio/{portfolioId}/transactions?filter=<BASE64> | references/apis.md |

---

# Transactions

Endpoint

GET /api/portfolio/{portfolioId}/transactions

Query params

page  
pageSize  
filter  

---

# Transaction Filters

Filters must be encoded with Base64 before sending.

---

## Transaction Hash

{"transactionHash":{"operator":"contains","value":"123456789"}}

---

## Transaction Date

Operator: between

{"transactionDate":{"operator":"between","value":{"startDate":"2026-03-01T00:00:00Z","endDate":"2026-03-02T23:59:59Z"}}}

---

## Transaction Type

Operator: contains_in

{"transactionType":{"operator":"contains_in","value":["computed-reward"]}}

Available transaction types:

- airdrop  
- bonus  
- computed-deposit  
- computed-reward  
- deposit  
- other  
- reward  
- staking  
- trade  
- unknown  
- unstaking  
- withdrawal  

---

# Combining Multiple Filters

Multiple filters can be applied in the **same JSON object**.

Example combining:

- transaction hash
- transaction type
- transaction date

Example JSON:

{
  "transactionHash": { "operator": "contains", "value": "123456" },
  "transactionType": { "operator": "contains_in", "value": ["trade","deposit"] },
  "transactionDate": {
    "operator": "between",
    "value": {
      "startDate": "2026-03-01T00:00:00Z",
      "endDate": "2026-03-31T23:59:59Z"
    }
  }
}

Encode this JSON to Base64 and pass it as:

GET /api/portfolio/{portfolioId}/transactions?filter=<BASE64>&page=1&pageSize=20

---

# Encoding Filters

Browser

btoa(JSON.stringify(filter))

Node

Buffer.from(JSON.stringify(filter)).toString("base64")

---

# Pagination

page starts at 1

pageSize default = 20

Continue requesting pages while:

items.length < total

---

# References

references/apis.md
