# DAX Ledger Skills

Agent Skills for integrating DAX Ledger APIs into AI-powered applications.

Capabilities:

- Authentication
- Portfolio listing
- Portfolio KPIs
- Portfolio findings
- Position snapshot (balances and values)
- Position snapshot graph by ticker
- DeFi positions
- Capital gains report
- Sanity check report
- Calculation summary report
- Fiscal report
- Transactions history
- Transaction filtering
- Multi-filter queries

Base URL

https://app.daxledger.io

---

# Skills

skills/daxledger-api

Entry point

skills/daxledger-api/SKILL.md

---

# Environment Variables

| Variable             | Description |
| -------------------- | ----------- |
| DAXLEDGER_API_KEY    | API key     |
| DAXLEDGER_API_SECRET | API secret  |

---

# Transactions

Endpoint

GET /api/portfolio/{portfolioId}/transactions

---

# Findings

Endpoints

GET /api/portfolio/{portfolioId}/findings?page=1&pageSize=20  
GET /api/portfolio/{portfolioId}/finding/{ruleId}

---

# Position Snapshot

Endpoints

GET /api/portfolio/{portfolioId}/position_snapshot?page=1&pageSize=20  
GET /api/portfolio/{portfolioId}/position_snapshot/graph/{ticker}?span=30
GET /api/portfolio/{portfolioId}/positions_report/defi

---

# Reports

Endpoints

GET /api/portfolio/{portfolioId}/capital_gains_report?page=1&pageSize=20  
GET /api/portfolio/{portfolioId}/sanity_check_report?page=1&pageSize=20  
GET /api/portfolio/{portfolioId}/calculation_summary_report?page=1&pageSize=20  
GET /api/portfolio/{portfolioId}/fiscal_report?page=1&pageSize=20

Supported filters

Transaction Hash  
Transaction Date  
Transaction Type

Filters must be Base64 encoded.

Example:

{"transactionType":{"operator":"contains_in","value":["computed-reward"]}}

---

# Combining Filters

Filters can be combined in the same JSON object.

Example:

{
"transactionHash": { "operator": "contains", "value": "abc123" },
"transactionType": { "operator": "contains_in", "value": ["trade"] },
"transactionDate": {
"operator": "between",
"value": {
"startDate": "2026-03-01T00:00:00Z",
"endDate": "2026-03-31T23:59:59Z"
}
}
}

Encode the JSON to Base64 and send it as:

GET /api/portfolio/{portfolioId}/transactions?filter=<BASE64>&page=1&pageSize=20

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

Fetch next page while items.length < total

---

# License

MIT
