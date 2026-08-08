# 🗑️ REM Domestic Skip Request

An **n8n workflow** that receives skip hire requests from a Tally form, scores each lead based on skip size, placement, and waste type, logs every lead to a Google Sheet, and prioritizes high-value opportunities with Slack alerts and a 24-hour follow-up reminder.

---

## 📋 Overview

Built for a domestic skip hire business, this workflow turns raw form submissions into prioritized, actionable leads:

1. A customer submits a skip request via a **Tally form**
2. The workflow extracts and cleans the submitted fields
3. A **lead score** is calculated based on:
   - Skip size (40-yard skips score higher — bigger jobs)
   - Placement (road/public placement scores higher — usually needs permits, higher-value jobs)
   - Waste type (mixed waste scores higher — typically pricier disposal)
4. Leads scoring **10 or above** are classified as **High Value**; everything else is **Normal**
5. **High Value leads** are saved to Google Sheets, trigger an immediate Slack alert to the sales team, and get a follow-up reminder in Slack 24 hours later if not yet closed
6. **Normal leads** are simply logged to the same Google Sheet for record-keeping

---

## 🏗️ Architecture

```
Receive Form (Webhook — Tally form submission)
        │
        ▼
Calculating Score (extract fields + compute lead score)
        │
        ▼
If (Score ≥ 10?)
        │
        ├── Yes → High Value (save to Sheet) → Send to Slack (alert) → Wait 24h → Follow Up (Slack reminder)
        └── No  → Normal (save to Sheet)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **Receive Form** | `webhook` | Receives the Tally form submission (`POST /REM-Waste`) |
| **Calculating Score** | `Set` | Extracts name, phone, email, address, skip size, placement, waste type, and date from the form payload; computes a numeric lead score |
| **If** | `if` | Branches leads into High Value (score ≥ 10) vs. Normal |
| **High Value** | `googleSheets` | Appends/updates the high-value lead's record in the Google Sheet |
| **Send to Slack** | `slack` | Posts an immediate "🔥 High-Value Lead Received" alert to the team's Slack channel |
| **Wait** | `wait` | Pauses 24 hours before sending the follow-up reminder |
| **Follow Up** | `slack` | Posts a "🔔 Follow-up Reminder" if the high-value lead hasn't been actioned within a day |
| **Normal** | `googleSheets` | Appends/updates normal-scoring leads in the same Google Sheet |

### Scoring Logic

| Factor | Condition | Points |
|---|---|---|
| Skip Size | Contains "40 yard" | +10 |
| Placement | Contains "road" or "public" | +5 |
| Waste Type | Contains "mixed waste" | +5 |

A total score of **10 or more** marks the lead as High Value.

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud) with a publicly reachable webhook URL
- A **Tally** form configured to POST submissions to the `/REM-Waste` webhook
- **Google Sheets** OAuth2 credentials + a sheet with columns: Name, Phone, Email, Address, Skip Size, Placement, Waste type, Date, Score
- A **Slack** app/bot token with permission to post to your target channel

---

## 🚀 Setup

1. Import `REM_Domestic_Skip_Request.json` into your n8n instance.
2. Activate the workflow and copy the **Receive Form** webhook URL.
3. In your Tally form settings, add a webhook integration pointing to that URL, matching the field order the workflow expects (name, phone, email, address, skip size, placement, waste type, date).
4. Configure Google Sheets and Slack credentials.
5. Point the **High Value**/**Normal** Google Sheets nodes and the **Send to Slack**/**Follow Up** nodes at your own sheet and channel.
6. Adjust the scoring thresholds/keywords in **Calculating Score** to match your own pricing or priority logic if needed.

---

## 🛠️ Customization Ideas

- Add a "Closed/Actioned" checkbox in the sheet and skip the 24-hour follow-up if it's already been marked done
- Add a second follow-up tier (e.g., 72 hours) for leads still unaddressed
- Send an automatic confirmation email/SMS to the customer upon submission
- Add more scoring factors (e.g., urgency, distance, repeat customer) for finer lead prioritization

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
