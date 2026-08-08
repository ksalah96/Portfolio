# 💰 Invoice Reminder Automation With AI Agent

An **n8n workflow** that checks Airtable daily for overdue invoices and automatically sends each client a polite, AI-drafted payment reminder email via Gmail.

---

## 📋 Overview

Every day at a scheduled time, the workflow pulls all invoice records from an Airtable base, filters for the ones that are past their due date, and hands each overdue invoice to an AI agent (Gemini 2.0 Flash) that drafts a professional, friendly reminder email — including the due date, today's date, days overdue, and amount owed. The email is then sent automatically via Gmail.

**Flow at a glance:**
1. ⏰ Daily trigger fires (8 AM)
2. 📋 Airtable is queried for all invoice records
3. 🔍 Records are filtered to only those past their due date
4. 🤖 AI Agent drafts a personalized reminder email
5. 📧 Email is sent to the client via Gmail

---

## 🏗️ Architecture

```
Daily Trigger
     │
     ▼
Airtable (search all invoice records)
     │
     ▼
If (Due_date < today?)
     │
     ▼
AI Agent (Gemini 2.0 Flash + Postgres memory) → drafts reminder email
     │
     ▼
Gmail (send email to client)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **Daily Trigger** | `scheduleTrigger` | Fires once daily (default: 8 AM) to kick off the check |
| **Airtable** | `airtable` | Searches the "Invoice Reminder" base for all invoice records |
| **If** | `if` | Filters records to only those whose `Due_date` is before today |
| **AI Agent** | `agent` | Drafts a professional reminder email using client name, due date, today's date, days overdue, and amount due |
| **Google Gemini Chat Model** | `lmChatGoogleGemini` | The LLM (Gemini 2.0 Flash) powering the AI Agent |
| **Postgres Chat Memory** | `memoryPostgresChat` | Keeps per-invoice conversation context, keyed by record ID |
| **Gmail** | `gmail` | Sends the AI-drafted reminder email to the client's address |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud)
- An **Airtable** base named "Invoice Reminder" with (at minimum) `Due_date`, `Amount_due`, `Client_name`, and `Email` fields, plus a Personal Access Token
- **Google Gemini (PaLM) API** credentials
- A **Postgres** database (for per-invoice chat memory)
- A **Gmail** account with OAuth2 credentials connected in n8n

---

## 🚀 Setup

1. Import `Invoice_Reminder_Automation_With_AI_Agent.json` into your n8n instance.
2. Configure credentials for Airtable, Google Gemini, Postgres, and Gmail.
3. Point the **Airtable** node at your own base and table, ensuring field names match (`Due_date`, `Amount_due`, `Client_name`, `Email`) or update the AI Agent's prompt references accordingly.
4. Adjust the **Daily Trigger** to your preferred check-in time.
5. Review the AI Agent's system prompt and tone to match your brand voice if needed.
6. Activate the workflow.

---

## 🛠️ Customization Ideas

- Add escalating reminder tiers (gentle nudge → firm reminder → final notice) based on days overdue
- CC or notify an internal finance/accounts-receivable channel (Slack/email) when a reminder is sent
- Log sent reminders back to Airtable (date sent, reminder count) to avoid duplicate sends
- Add a "payment received" webhook to automatically stop further reminders for a given invoice

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
