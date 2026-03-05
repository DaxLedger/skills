
# DAX Ledger API Reference

Base URL  
https://app.daxledger.io

---

# Authentication

POST /api/auth/external_api

---

# List Portfolios

GET /api/portfolios

---

# Portfolio KPIs

GET /api/portfolio/{portfolioId}/kpis/portfolio

---

# Transactions

GET /api/portfolio/{portfolioId}/transactions

Query parameters:

page  
pageSize  
filter  

---

# Filters

## Transaction Hash

{"transactionHash":{"operator":"contains","value":"123456789"}}

---

## Transaction Date

{"transactionDate":{"operator":"between","value":{"startDate":"2026-03-01T00:00:00Z","endDate":"2026-03-02T23:59:59Z"}}}

---

## Transaction Type

{"transactionType":{"operator":"contains_in","value":["computed-reward"]}}

Available values:

airdrop  
bonus  
computed-deposit  
computed-reward  
deposit  
other  
reward  
staking  
trade  
unknown  
unstaking  
withdrawal  

---

# Combining Filters

Multiple filters can be combined in the same JSON object before encoding.

Example:

{
  "transactionHash": { "operator": "contains", "value": "123456" },
  "transactionType": { "operator": "contains_in", "value": ["trade"] },
  "transactionDate": {
    "operator": "between",
    "value": {
      "startDate": "2026-03-01T00:00:00Z",
      "endDate": "2026-03-31T23:59:59Z"
    }
  }
}

Encode the JSON to Base64 before sending it as the `filter` query parameter.

---

# Encoding

Browser:

btoa(JSON.stringify(filter))

Node:

Buffer.from(JSON.stringify(filter)).toString("base64")

---

# Pagination

page starts at 1

pageSize default = 20

Continue requesting pages while items.length < total
