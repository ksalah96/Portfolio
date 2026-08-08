# 💊 PharmaLead Scraper

An **n8n workflow** that scrapes pharmaceutical company listings from Google Places (via Apify) and logs them into a Google Sheet — built as a lightweight lead sourcing tool for targeted B2B outreach.

---

## 📋 Overview

Run on demand, this workflow triggers an Apify actor to search Google Places for pharmaceutical companies in a given location (currently configured for Riyadh, Saudi Arabia), waits for the scrape to complete, retrieves the results, and appends each company's details to a Google Sheet for follow-up.

**Flow at a glance:**
1. ▶️ Manual trigger starts the run
2. 🕷️ Apify's Google Places crawler actor is triggered with search parameters
3. ⏳ Workflow waits for the scrape to finish
4. 📥 Results are retrieved from the Apify dataset
5. 📊 Company details are appended to a Google Sheet

---

## 🏗️ Architecture

```
Manual Trigger
     │
     ▼
Run Apify (start Google Places scrape)
     │
     ▼
Wait (25s — allow scrape to complete)
     │
     ▼
Get Apify (fetch dataset results)
     │
     ▼
Google Sheets (append company records)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **When clicking 'Execute workflow'** | `manualTrigger` | Manually starts the scrape |
| **Run Apify** | `httpRequest` | Triggers the Apify `compass~crawler-google-places` actor with search filters (category: pharmaceutical company, location: Riyadh, min. 4-star rating, contact scraping enabled) |
| **Wait** | `wait` | Pauses 25 seconds to give the Apify run time to complete before fetching results |
| **Get Apify** | `httpRequest` | Retrieves the scraped dataset items from the most recent Apify run |
| **Google Sheets** | `googleSheets` | Appends each company's name, address, phone, website, and rating to a spreadsheet |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud)
- An **Apify** account with access to the `compass~crawler-google-places` actor, and an API token
- **Google Sheets** OAuth2 credentials + a sheet with columns: Name, Address, Phone, Website, Rate

> ⚠️ **Security note — action required before publishing:** The Apify API token is embedded directly in the request URL's query string (`?token=***`) in both the **Run Apify** and **Get Apify** nodes, rather than being masked as a header credential. Even though it's shown as `***` here, double-check your actual exported file for the real token before pushing to GitHub. Since Apify tokens in URLs are easy to accidentally leak (e.g., in logs, browser history, or shared screenshots), it's best practice to:
> 1. Move the token into n8n's credential store instead of the URL
> 2. Rotate the token if it has ever been shared or committed anywhere in plain text

---

## 🚀 Setup

1. Import `PharmaLead Scraper.json` into your n8n instance.
2. Add your **Apify** API token (ideally via n8n credentials rather than inline in the URL — see security note above).
3. Point the **Google Sheets** node at your own spreadsheet with matching columns.
4. Adjust the **Run Apify** node's `locationQuery`, `categoryFilterWords`, and `searchStringsArray` to target a different city, industry, or rating threshold.
5. Run manually via the trigger.

---

## 🛠️ Customization Ideas

- Replace the fixed 25-second **Wait** with a polling loop that checks Apify run status before fetching results, for more reliability on larger scrapes
- Add deduplication against existing sheet rows before appending
- Chain in an email/CRM push step (similar to the Apollo Lead Gen workflow) to move scraped leads directly into outreach
- Parameterize the location and category via a form trigger so non-technical users can run custom searches

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
