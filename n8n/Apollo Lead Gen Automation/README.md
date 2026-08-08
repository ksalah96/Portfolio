# 🎯 Apollo Lead Gen Automation

An **n8n workflow** that turns a plain-English prospecting request into a fully verified, enriched lead list — from natural language, to an Apollo.io search URL, to scraped contacts, to verified emails, to a Google Sheet, to an active cold email campaign.

---

## 📋 Overview

Instead of manually building Apollo.io search filters, scraping results, and verifying emails one by one, this workflow automates the entire lead-generation pipeline:

1. You describe your ideal lead in plain language (e.g. *"Automation Developer in Egypt"*)
2. **Gemini 2.5 Flash** converts that into a structured Apollo.io search URL (location, titles, keywords)
3. **Apify** scrapes the resulting Apollo search for matching people and their emails
4. Results are limited to a set batch size and filtered to only records with an email
5. Each email is checked for deliverability via a real-time **email verification API**
6. Valid, verified leads are added to a **Google Sheet**
7. Verified leads are automatically pushed into a **Smartlead** cold email campaign

---

## 🏗️ Architecture

```
Manual Trigger
     │
     ▼
Apollo URL Maker (Gemini — NL query → Apollo search URL)
     │
     ▼
Clean URL (parse JSON response)
     │
     ▼
Apify (scrape Apollo search results + emails)
     │
     ▼
Limit (cap number of leads)
     │
     ▼
Email Exist? ──(no)──► No Operation
     │ (yes)
     ▼
Loop Over Items
     │
     ▼
Check Email (Reoon email verification)
     │
     ▼
Wait (rate-limit pause)
     │
     ▼
Check Valid Email? ──(no)──► No Operation
     │ (yes)
     ▼
Add to Sheet (Google Sheets)
     │
     ▼
Cold Email Campaign (Smartlead)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **When clicking 'Execute workflow'** | `manualTrigger` | Manually starts the pipeline with a natural-language query |
| **Apollo URL Maker** | `googleGemini` | Converts a plain-English lead description into a structured Apollo.io search URL (location, titles, keywords) |
| **Clean URL** | `Set` | Parses the Gemini JSON response into a usable `searchURL` field |
| **Apify** | `httpRequest` | Runs an Apify actor that scrapes the Apollo search results, including personal/work emails |
| **Limit** | `limit` | Caps the number of leads processed per run |
| **Email Exist** | `if` | Filters out any scraped records that have no email address |
| **Loop Over Items** | `splitInBatches` | Processes leads one at a time for verification |
| **Check Email** | `httpRequest` | Verifies email deliverability via the Reoon Email Verifier API |
| **Wait** | `wait` | Pauses between verification calls to respect API rate limits |
| **Check Valid Email** | `if` | Only proceeds if the verifier returns a `valid` status |
| **Add to Sheet** | `googleSheets` | Appends/updates the verified lead (name, title, LinkedIn URL, email) in a Google Sheet |
| **Cold Email Campaign** | `httpRequest` | Adds the verified lead into a Smartlead cold email campaign |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud)
- **Google Gemini (PaLM) API** credentials
- An **Apify** account with access to an Apollo scraper actor, and an API token
- A **Reoon Email Verifier** API key
- **Google Sheets** OAuth2 credentials + a sheet with columns: ID, First Name, Last Name, LinkedinURL, Title, Email
- A **Smartlead** account with an active campaign ID and API key

> ⚠️ **Security note — action required before publishing:** This workflow JSON currently has a **live Reoon email verification API key hardcoded in plain text** in the "Check Email" node's URL (`key=Oo9YLLvRNxRbs2TNMv5efDmnNUslF4y5`). The Apify and Smartlead keys are already masked as `***`, but the Reoon key is fully exposed. Before pushing this to GitHub:
> 1. Rotate/regenerate the Reoon API key from your account dashboard
> 2. Replace the key in the URL with a placeholder (e.g. `key=YOUR_REOON_API_KEY`) or move it into n8n credentials
> 3. Double check the Apify Authorization header and Smartlead `api_key` query parameter are truly redacted (currently shown as `Bearer ***` / `***`) and not your real keys before committing

---

## 🚀 Setup

1. Import `Apollo Lead Gen Automation.json` into your n8n instance.
2. Configure credentials for Google Gemini, Apify, Google Sheets, and add the Reoon and Smartlead keys securely (see security note above).
3. Update the **Apify** node's actor URL/ID if you're using a different Apollo scraper actor.
4. Point the **Add to Sheet** node at your own Google Sheet with matching columns.
5. Update the **Cold Email Campaign** node with your own Smartlead campaign ID.
6. Run manually via the trigger, entering your target lead description (e.g. *"Marketing agencies in Dubai"*).

---

## 🛠️ Customization Ideas

- Replace the manual trigger with a chat trigger or form so non-technical teammates can request lead lists
- Add deduplication against existing Google Sheet rows before appending new leads
- Add a Slack notification summarizing how many leads were found/verified per run
- Extend the AI URL Maker prompt to support company size, industry, or funding-stage filters

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
