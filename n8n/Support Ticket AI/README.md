# 🎫 Support Ticket AI

An **n8n workflow** that turns incoming support emails into structured, categorized tickets — using **Google Gemini** to extract the key details and auto-route each ticket to the right team in **Notion**.

---

## 📋 Overview

Every unread email in a monitored inbox is treated as a potential support ticket. The workflow reads the email body, sends it to Gemini for analysis, and extracts a clean structured record: a short summary, urgency level, category, and the client's name and email. Based on the category and urgency, the ticket is automatically assigned to the right team, then logged into a Notion "Support Tickets" database.

**Flow at a glance:**
1. 📥 New unread email arrives (via IMAP)
2. 🧹 Email body is cleaned/extracted
3. 🤖 AI Agent analyzes the email and returns structured JSON (summary, urgency, category, client name/email)
4. 🔀 Ticket is auto-routed based on category/urgency
5. 📝 Ticket is created in Notion

---

## 🏗️ Architecture

```
Email Trigger (IMAP, unseen only)
        │
        ▼
Clean Body
        │
        ▼
Basic LLM Chain (Gemini — extract summary, urgency, category, client info)
        │
        ▼
Extract (parse AI JSON + apply auto-routing logic)
        │
        ▼
Notion (create ticket page)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **Email Trigger (IMAP)** | `emailReadImap` | Polls the inbox via IMAP for unread messages |
| **Clean Body** | `Set` | Extracts the HTML (or plain-text fallback) body from the incoming email |
| **Basic LLM Chain** | `chainLlm` | Sends the email content to Gemini, which returns structured JSON: summary, urgency (low/medium/high), category (billing/technical/general), client email, client name |
| **Google Gemini Chat Model** | `lmChatGoogleGemini` | The LLM (Gemini 2.0 Flash) powering the extraction |
| **Extract** | `Set` | Parses the AI's JSON output and applies auto-routing logic (see below) |
| **Notion** | `notion` | Creates a new page in the "Support Tickets" database with all extracted fields |

### Auto-Routing Logic

| Condition | Assigned To |
|---|---|
| Category = `technical` | Engineer |
| Category = `billing` | Finance |
| Category = `general` and Urgency = `high` | Manager |
| Anything else / unrecognized | Support (default) |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud)
- An email account with **IMAP** access configured (e.g. Gmail IMAP)
- **Google Gemini (PaLM) API** credentials
- A **Notion** integration with access to a "Support Tickets" database containing `Summary` (title), `Urgency` (select), `Category` (select), `Client Email` (email), `Client Name` (rich text), and `Assigned To` (rich text) properties

> ⚠️ **Privacy note — action required before publishing:** This workflow's exported JSON includes **pinned test data** in the `pinData` section containing a real sample email exchange — including personal Gmail/Yahoo addresses, full raw email headers, DKIM signatures, and message IDs. Before pushing this to a public GitHub repo:
> 1. Remove the `pinData` block entirely, or replace it with a synthetic/dummy example email
> 2. Double check no other personal email addresses remain in the file

---

## 🚀 Setup

1. Import `Support_Ticket_AI.json` into your n8n instance.
2. Configure credentials for IMAP, Google Gemini, and Notion.
3. Remove or replace the pinned test email data (see privacy note above).
4. Point the **Notion** node at your own "Support Tickets" database, matching the property names/types listed above.
5. Adjust the auto-routing logic in the **Extract** node if your team structure differs.
6. Activate the workflow.

---

## 🛠️ Customization Ideas

- Send an automatic acknowledgment reply to the customer confirming their ticket was received
- Add a Slack notification for high-urgency tickets so the right team is alerted immediately
- Track SLA timers per urgency level and escalate if a ticket goes unanswered too long
- Extend categories (e.g., "onboarding," "feature request") as support volume grows

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
