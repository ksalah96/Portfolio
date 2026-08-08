# 🔍 Website Tech Scraper

An **n8n workflow** that reads a list of company domains from Google Sheets, scrapes their tech stack from BuiltWith, uses **Google Gemini** to classify the technologies into clean categories, and writes the structured results back to the same sheet — a lightweight, self-hosted alternative to tools like Clay for tech-stack enrichment.

---

## 📋 Overview

Given a spreadsheet of company domains, this workflow enriches each row with the technologies that company's website uses — hosting, CMS, analytics, security, and more — fully automated:

1. Company domains are read from a Google Sheet
2. Each domain is processed one at a time in a loop
3. The domain's BuiltWith profile is fetched (via the Jina AI Reader proxy, which returns clean readable page content)
4. **Gemini** analyzes the raw page content and extracts only the technology names, organized into categories
5. A structured output parser enforces a consistent JSON schema for the categorized results
6. The categorized tech stack is written back to the same Google Sheet, matched by domain

---

## 🏗️ Architecture

```
Manual Trigger
     │
     ▼
Get row(s) in sheet (Google Sheets — list of company domains)
     │
     ▼
Loop Over Items ──────────────────────────────┐
     │                                         │
     ▼                                         │
Tech Technologies Sourcing                     │
(fetch BuiltWith page via Jina AI Reader)      │
     │                                         │
     ▼                                         │
Basic LLM Chain (Gemini + Structured Output Parser)
     │                                         │
     ▼                                         │
Add Information (write categorized tech stack back to sheet)
     │                                         │
     └─────────────────► loops back ───────────┘
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **When clicking 'Execute workflow'** | `manualTrigger` | Manually starts the enrichment run |
| **Get row(s) in sheet** | `googleSheets` | Reads the list of company domains to process |
| **Loop Over Items** | `splitInBatches` | Processes each domain one at a time |
| **Tech Technologies Sourcing** | `httpRequest` | Fetches the company's BuiltWith page content via the Jina AI Reader proxy (`r.jina.ai`) |
| **Basic LLM Chain** | `chainLlm` | Sends the scraped content to Gemini to extract and categorize the technologies used |
| **Google Gemini Chat Model** | `lmChatGoogleGemini` | The LLM (Gemini) powering the technology extraction |
| **Structured Output Parser** | `outputParserStructured` | Enforces a consistent JSON schema (Hosting & Servers, CMS & Content Management, Analysis & Tracking Tools, Security & Performance, Other Technologies) |
| **Add Information** | `googleSheets` | Appends/updates the row for that domain with the categorized tech stack |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud)
- **Google Sheets** OAuth2 credentials + a sheet with a `Company Domain` column (and the output columns: Hosting & Server, CMS & Content Management, Analysis & Tracking Tools, Security & Performance, Other Technologies)
- A **Jina AI Reader** API token (used to fetch clean, readable page content from BuiltWith)
- **Google Gemini (PaLM) API** credentials

---

## 🚀 Setup

1. Import `Website_Tech_Scraper.json` into your n8n instance.
2. Configure credentials for Google Sheets and Google Gemini.
3. Add your **Jina AI Reader** bearer token in the **Tech Technologies Sourcing** node's Authorization header.
4. Point the **Get row(s) in sheet** and **Add Information** nodes at your own spreadsheet, ensuring the `Company Domain` column matches.
5. Run manually via the trigger — the workflow will process every row currently in the sheet.

---

## 🛠️ Customization Ideas

- Add a "Processed" flag/date column and skip rows already enriched, so re-runs only pick up new domains
- Add error handling for domains with no BuiltWith data (e.g., very small or new sites)
- Swap BuiltWith for another tech-detection source, or combine multiple sources for more coverage
- Add a rate-limit delay between iterations if scraping large domain lists to avoid throttling

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
