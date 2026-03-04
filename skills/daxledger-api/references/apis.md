# DAX Ledger API Reference (Minimal + Transactions)

Base URL:
https://app.daxledger.io

Authentication uses a Bearer JWT token.

---

# 1 Authentication

## POST /api/auth/external_api

Request:

{
  "APIKey": "string",
  "APISecret": "string"
}

Response:

{
  "token": "your_access_token_here"
}

Example:

curl -X POST https://app.daxledger.io/api/auth/external_api \
-H "Content-Type: application/json" \
-d '{
  "APIKey": "YOUR_API_KEY",
  "APISecret": "YOUR_API_SECRET"
}'

---

# 2 List Portfolios

## GET /api/portfolios

Header:

Authorization: Bearer <token>

Optional query:

show=owned | shared

Example:

curl https://app.daxledger.io/api/portfolios?show=owned \
-H "Authorization: Bearer YOUR_TOKEN"

---

# 3 Portfolio KPIs

## GET /api/portfolio/{portfolioId}/kpis/portfolio

Header:

Authorization: Bearer <token>

Example:

curl https://app.daxledger.io/api/portfolio/123/kpis/portfolio \
-H "Authorization: Bearer YOUR_TOKEN"

Note:
The API spec defines the KPI list as `items`, but examples may return `kpis`.
Robust parsing should support both:

const kpis = response.items ?? response.kpis ?? []

---

# 4 Transactions (List + Filters + Pagination)

## GET /api/portfolio/{portfolioId}/transactions

Headers:
- Authorization: Bearer <token>

Query parameters:
- page (integer): starts at 1
- pageSize (integer): default 20
- filter (string): Base64-encoded JSON filter string

### Filter: Transaction Hash (contains)

Raw filter JSON string:
{"transactionHash":{"operator":"contains","value":"123456789"}}

Encode and pass as query param `filter`.

Browser:
btoa('{"transactionHash":{"operator":"contains","value":"123456789"}}')

Node.js:
Buffer.from('{"transactionHash":{"operator":"contains","value":"123456789"}}', 'utf8').toString('base64')

Example request:

curl "https://app.daxledger.io/api/portfolio/${PORTFOLIO_ID}/transactions?page=1&pageSize=20&filter=${FILTER_B64}" \
  -H "Authorization: Bearer ${TOKEN}"

### Pagination iteration rule

- Fetch page=1
- Collect items
- While collectedItems.length < total, increment page and fetch next page
- Stop when collectedItems.length >= total

Note:
Some responses may nest the list as `response.items.items`.
Robust parsing should support:
- items = response.items (if array)
- items = response.items.items (if nested)
- total = response.total OR response.items.total
