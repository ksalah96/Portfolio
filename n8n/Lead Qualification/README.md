# 🏠 Real Estate Lead Qualification Automation

An **n8n workflow** that monitors an inbox for incoming rental inquiries, uses **Google Gemini** to qualify each lead against business criteria, and automatically routes qualified leads into **HubSpot** while flagging skipped leads to a **Slack** channel.

---

## 📋 Overview

Built for a real estate company renting apartments in **Riyadh and Jeddah** (priced under 800 SAR/day), this workflow removes manual lead triage from the inbox. Every unread email is read, cleaned, and passed to an AI qualification agent that checks for:

- Genuine interest in apartment rentals
- Location match (Riyadh or Jeddah)
- Budget alignment (<800 SAR/day)

The AI extracts the sender's name, email, phone, and company, and returns a structured `is_qualified` verdict with a one-sentence reason. Qualified leads are pushed straight into HubSpot as new contacts; unqualified leads are logged to Slack for team visibility (with the reason for skipping).

**Flow at a glance:**
1. 📥 New unread email arrives
2. 🧹 Email body is cleaned/extracted
3. 🤖 AI Agent analyzes and qualifies the lead (JSON output)
4. 🔀 Branch on qualification status:
   - ✅ **Qualified** → parsed and added to HubSpot as a contact
   - ❌ **Not qualified** → parsed and posted to Slack with the reason

---

## 🏗️ Architecture

```
Receive Email (IMAP, unseen only)
        │
        ▼
Clean Email Body
        │
        ▼
Basic LLM Chain (Gemini 2.0 Flash — qualification analysis)
        │
        ▼
Qualified? (checks for "is_qualified": true)
        │
        ├── Yes → Extract → HubSpot (create/update contact)
        └── No  → Extract1 → Slack (post skipped-lead notice)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **Receive Email** | `emailReadImap` | Polls the inbox via IMAP for unread messages |
| **Clean Email Body** | `Set` | Extracts plain text (or HTML fallback) from the incoming email |
| **Basic LLM Chain** | `chainLlm` | Sends the email text to Gemini for qualification analysis, returning structured JSON |
| **Google Gemini Chat Model** | `lmChatGoogleGemini` | The LLM (Gemini 2.0 Flash) powering the qualification chain |
| **Qualified?** | `if` | Branches based on whether the AI response contains `"is_qualified": true` |
| **Extract** | `Set` | Parses the AI's JSON output (name, email, phone, qualification, reason) for qualified leads |
| **Extract1** | `Set` | Parses the same fields for unqualified leads |
| **HubSpot** | `hubspot` | Creates/updates a contact record for qualified leads |
| **Slack** | `slack` | Posts a "Lead Skipped" notice with the reason and email for unqualified leads |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud)
- An email account with **IMAP** access configured (e.g. Gmail IMAP)
- **Google Gemini (PaLM) API** credentials
- A **HubSpot** account with an App Token and CRM access
- A **Slack** app/bot token with permission to post to your target channel

---

## 🚀 Setup

1. Import `Real_Estate_Lead_Qualification_Automation.json` into your n8n instance.
2. Configure credentials for IMAP, Google Gemini, HubSpot, and Slack.
3. Update the **Basic LLM Chain** system prompt if your qualification criteria differ (region, price threshold, property type).
4. Point the **Slack** node's channel ID to your own team channel.
5. Confirm your **HubSpot** App Token has contact create/update permissions.
6. Activate the workflow.

---

## 🛠️ Customization Ideas

- Add a fallback branch for leads the AI can't confidently classify, routing them to a human for manual review
- Log every processed lead (qualified or not) to a spreadsheet for reporting/analytics
- Add automatic reply emails to qualified leads with next steps or a booking link
- Extend qualification criteria to additional cities or price tiers as the business grows

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
