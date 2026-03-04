# DAX Ledger Skills

Agent Skills for integrating **DAX Ledger portfolio APIs** into AI-powered applications.

These skills allow agents to authenticate with the DAX Ledger API, list portfolios, and retrieve portfolio KPIs.

---

# Skills

## skills/daxledger-api

Query portfolio information and analytics from the DAX Ledger platform.

Capabilities include:

- Authenticating with the DAX Ledger API
- Listing available portfolios
- Retrieving portfolio KPIs
- Accessing portfolio analytics data

Auth: APIKey + APISecret exchanged for a JWT token  
Base URL: https://app.daxledger.io

Entry point:

skills/daxledger-api/SKILL.md

---

# Which skill should I use?

| Scenario | Skill |
|--------|--------|
| I need to authenticate with the DAX Ledger API | daxledger-api |
| I need to list available portfolios | daxledger-api |
| I need to retrieve KPIs for a portfolio | daxledger-api |

---

# Environment Variables

The skill requires the following environment variables:

| Variable | Description |
|--------|-------------|
| DAXLEDGER_API_KEY | API key used to authenticate with DAX Ledger |
| DAXLEDGER_API_SECRET | API secret used together with the API key |

These credentials are exchanged for a **JWT token** via the authentication endpoint.

---

# Installation

If the skill is published in a skills registry:

npx skills add https://github.com/DaxLedger/skills --yes

Or manually copy the folder into your project:
```
.
├── README.md
└── skills
    └── daxledger-api
        ├── references
        │   └── apis.md
        └── SKILL.md
```
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

# Specification

This skill follows the **Agent Skills specification** used for LLM tool integration.

The specification defines how agents:

- discover available APIs
- choose endpoints
- structure requests
- interpret responses

---

# API Reference

Detailed endpoint documentation is available in:

skills/daxledger-api/references/apis.md

---

# License

MIT
