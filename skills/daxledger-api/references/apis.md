
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

# Findings

## List Findings

GET /api/portfolio/{portfolioId}/findings

Query parameters:

page  
pageSize  

---

## Finding By Rule Id

GET /api/portfolio/{portfolioId}/finding/{ruleId}


---

# Position Snapshot

## Positions Snapshot (balances and values)

GET /api/portfolio/{portfolioId}/position_snapshot

Query parameters:

page  
pageSize  
filter  
sort  

---

## Position Snapshot Graph By Ticker

GET /api/portfolio/{portfolioId}/position_snapshot/graph/{ticker}

Query parameters:

span
- 7: 7 days interval
- 30: 30 days interval
- 365: 365 days interval
- -1: all range available

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

# Epoch Fields

If an API field is an epoch timestamp, convert it to ISO date before returning it to the user.

Rule:

- 10 digits -> seconds
- 13 digits -> milliseconds

Example (Node):

new Date(Number(epoch) < 1e12 ? Number(epoch) * 1000 : Number(epoch)).toISOString()

---

# Pagination

page starts at 1

pageSize default = 20

Continue requesting pages while items.length < total
