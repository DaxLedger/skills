# DAX Ledger API Reference

Base URL\
https://app.daxledger.io

---

# Authentication

## POST /api/auth/external_api

### Request Example

```json
{
  "APIKey": "your_api_key",
  "APISecret": "your_api_secret"
}
```

### Response

```json
{
  "token": "your_access_token_here"
}
```

Authorization header for authenticated requests:

Authorization: Bearer `<token>`

---

# List Portfolios

## GET /api/portfolios

### Request Example

GET https://app.daxledger.io/api/portfolios Authorization: Bearer
`<token>`

---

# Portfolio KPIs

## GET /api/portfolio/{portfolioId}/kpis/portfolio

### Request Example

GET https://app.daxledger.io/api/portfolio/123/kpis/portfolio
Authorization: Bearer `<token>`

---

# Findings

## List Findings

GET /api/portfolio/{portfolioId}/findings

Query parameters:

page\
pageSize

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/findings?page=1&pageSize=20
Authorization: Bearer `<token>`

---

## Finding By Rule Id

GET /api/portfolio/{portfolioId}/finding/{ruleId}

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/finding/001
Authorization: Bearer `<token>`

---

# Position Snapshot

## Positions Snapshot

GET /api/portfolio/{portfolioId}/position_snapshot

Query parameters:

page\
pageSize\
filter\
sort

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/position_snapshot?page=1&pageSize=20
Authorization: Bearer `<token>`

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

## DeFi Positions

GET /api/portfolio/{portfolioId}/positions_report/defi

### Request Example

GET https://app.daxledger.io/api/portfolio/123/positions_report/defi
Authorization: Bearer `<token>`

---

# Reports

## Capital Gains Report

GET /api/portfolio/{portfolioId}/capital_gains_report

Query parameters:

page\
pageSize\
filter\
sort

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/capital_gains_report?page=1&pageSize=20
Authorization: Bearer `<token>`

### Available Filters

- year (operator: '=')
- digitalAssetTicker (operator: 'contains_in')
- portfolioConnection (operator: '=')

---

## Capital Gains Available Filters

GET /api/portfolio/{portfolioId}/capital_gains_report/filters

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/capital_gains_report/filters
Authorization: Bearer `<token>`

---

## Sanity Check Report

GET /api/portfolio/{portfolioId}/sanity_check_report

Query parameters:

page\
pageSize\
filter

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/sanity_check_report?page=1&pageSize=20
Authorization: Bearer `<token>`

### Available Filters

- portfolioConnectionName (operator: 'contains_in')
- digitalAssetTicker (operator: 'contains_in')
- ignoreSpamTokens (operator: '=', value: true or false)

---

## Calculation Summary Report

GET /api/portfolio/{portfolioId}/calculation_summary_report

Query parameters:

page\
pageSize\
filter\
sort

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/calculation_summary_report?page=1&pageSize=20
Authorization: Bearer `<token>`

### Available Filters

- portfolioConnection (operator: '=')
- year (operator: '=')

---

## Fiscal Report

GET /api/portfolio/{portfolioId}/fiscal_report

Query parameters:

page\
pageSize\
filter\
sort

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/fiscal_report?page=1&pageSize=20
Authorization: Bearer `<token>`

### Available Filters

- portfolioConnectionName (operator: 'contains_in')
- digitalAssetTicker (operator: 'contains_in')
- saleDate (operator: 'between', value: startDate and endDate)
- acquisitionDate (operator: 'between', value: startDate and endDate)
- holdingTerm (operator: '=', value: short-term, long-term, all)

---

# Transactions

GET /api/portfolio/{portfolioId}/transactions

Query parameters:

page\
pageSize\
filter

### Request Example

GET
https://app.daxledger.io/api/portfolio/123/transactions?page=1&pageSize=20
Authorization: Bearer `<token>`

### Available Filters

- transactionHash (operator: 'contains')
- address (operator: 'contains_in')
- portfolioConnectionName (operator: 'contains_in')
- digitalAssetTicker (operator: 'contains_in')
- transactionType (operator: 'contains_in')
- transactionDate (operator: 'between', value: startDate and endDate)

---

# Encoding

Browser:

btoa(JSON.stringify(filter))

Node:

Buffer.from(JSON.stringify(filter)).toString("base64")

---

# Epoch Fields

If an API field is an epoch timestamp, convert it to ISO date before
returning it.

Rule:

10 digits -\> seconds 13 digits -\> milliseconds

Example:

new Date(Number(epoch) \< 1e12 ? Number(epoch) \* 1000 :
Number(epoch)).toISOString()

---

# Pagination

page starts at 1

pageSize default = 20

Continue requesting pages while:

items.length < total
