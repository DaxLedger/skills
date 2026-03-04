# DAX Ledger API Reference (Minimal)

Base URL:
https://app.daxledger.io

Authentication uses a Bearer JWT token.

---

# Authentication

POST /api/auth/external_api

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

# List Portfolios

GET /api/portfolios

Header:

Authorization: Bearer <token>

Optional query:

show=owned | shared

Example:

curl https://app.daxledger.io/api/portfolios?show=owned \
-H "Authorization: Bearer YOUR_TOKEN"

---

# Portfolio KPIs

GET /api/portfolio/{portfolioId}/kpis/portfolio

Header:

Authorization: Bearer <token>

Example:

curl https://app.daxledger.io/api/portfolio/123/kpis/portfolio \
-H "Authorization: Bearer YOUR_TOKEN"

Example response:

{
"kpis": [
{
"id": 14,
"portfolioId": 5,
"kpiName": "totalBalance",
"kpiValue": {
"type": "currency",
"value": 1312.026208674356
},
"kpiType": "kpi"
}
]
}
