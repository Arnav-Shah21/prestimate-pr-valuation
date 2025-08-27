This repository outlines the workflow architecture of “Prestimate,” an internal tool created as part of an internship at Imperium AI. It calculates a company’s PR value using SEO metrics, public signals, and financial data. Built entirely in n8n, this tool automates API calls and valuations based on a proprietary formula.

Note: Due to the confidential nature of this internship project, no source code or exported workflows are included in this repo. The README outlines the conceptual framework and implementation steps.

## 1) Clone the repository

git clone https://github.com/<your-username>/prestimate-pr-valuation.git
cd prestimate-pr-valuation

---

## 2) Set up Python environment (for local docs, optional)

python3 -m venv venv
source venv/bin/activate     # Windows: venv\Scripts\activate
pip install -r requirements.txt

---

## 3) Objective

Build a backend workflow using n8n that:
- Accepts company name, domain, and ticker input
- Queries public data via APIs (SEMrush, Yahoo Finance)
- Computes a monthly PR value in USD
- Returns structured results to the frontend or dashboard

---

## 4) API Services Used

- **SEMrush API** for domain authority and SEO metrics  
- **Yahoo Finance API** for financials (market cap, revenue)  
- Optional integrations (not implemented): LinkedIn/Twitter followers, press article detection, etc.

---

## 5) Sample Formula

Prestimate ($/month) =  
3000 × (# of Tier-1 press releases) +  
1000 × (# of Tier-2 organic articles) +  
0.02 × (LinkedIn followers) +  
0.01 × (Twitter followers) +  
5 × (# of Referring Domains) +  
500 × (SEMrush Authority Score)

---

## 6) Step-by-Step n8n Workflow Setup

- Drag in a **Webhook node** to accept client input (domain + ticker)
- Add an **HTTP Request node** to call SEMrush API
- Add another **HTTP Request node** for Yahoo Finance (unofficial API)
- Add **Function node** to apply the scoring formula
- Use **Set node** to format the JSON output
- Add a **Response node** to return the score to the client/dashboard

---

## 7) SEMrush API Details

- Endpoint: `https://api.semrush.com/`
- Required: API key + domain
- Metrics pulled:
  - Authority Score
  - Organic Traffic
  - Referring Domains
  - Paid Search Spend

---

## 8) Yahoo Finance API Details

- Accessed via `https://query1.finance.yahoo.com/v10/finance/quoteSummary/<ticker>`
- Data pulled:
  - Market Cap
  - Revenue (TTM)
  - P/E Ratio
  - Sector / Industry

---

## 9) Workflow Output (Sample)

{
  "company": "Netflix",
  "prestimate_score": 18700,
  "inputs": {
    "authority_score": 58,
    "ref_domains": 410,
    "linkedin_followers": 180000,
    ...
  }
}

---

## 10) Deployment

- Hosted on Imperium's **private self-hosted n8n**
- Integrated into CRM via Webhook
- OAuth authentication restricted to internal users only

---

## 11) Confidentiality Statement

This repo documents a confidential project done during an internship. No proprietary source code, exports, or workflow images are included.

