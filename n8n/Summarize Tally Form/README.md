# 📝 Summarize Tally Form

An **n8n workflow** that captures event feedback from a Tally form, logs it to Google Sheets, and uses **Google Gemini** to turn the raw responses into a polished, stakeholder-ready summary email — delivered straight to your inbox.

---

## 📋 Overview

After an event, attendees fill out a Tally feedback form with three key questions: what went great, how the event could improve, and how likely they are to recommend it. This workflow automates turning that raw feedback into an actionable report:

1. Form submission is received via webhook and logged to Google Sheets
2. The freshly logged data is read back and aggregated across the three feedback fields
3. **Gemini 2.0 Flash** analyzes the responses and drafts a professional Markdown summary email (positive highlights, areas for improvement, recommendation insight, and a wrap-up)
4. The Markdown is converted to HTML
5. The finished summary is emailed out automatically

---

## 🏗️ Architecture

```
Post Tally Form (Webhook)
        │
        ▼
Send Form to Google Sheet (append raw response)
        │
        ▼
Get Raw from Google Sheet (read back logged data)
        │
        ▼
Aggregate (combine feedback fields into one record)
        │
        ▼
Basic LLM Chain (Gemini — generate Markdown summary email)
        │
        ▼
Markdown (convert Markdown → HTML)
        │
        ▼
Gmail (send the summary email)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **Post Tally Form** | `webhook` | Receives the Tally form submission (`POST /Salah-concert`) |
| **Send Form to Google Sheet** | `googleSheets` | Appends the submitter's name, email, phone, and answers to a spreadsheet |
| **Get Raw from Google Sheet** | `googleSheets` | Reads the data back out of the sheet for processing |
| **Aggregate** | `aggregate` | Combines the three feedback fields ("What went great?", "How can we improve?", "Recommendation likelihood") into a single record |
| **Basic LLM Chain** | `chainLlm` | Sends the aggregated feedback to Gemini, which produces a structured Markdown summary formatted as an email |
| **Google Gemini Chat Model** | `lmChatGoogleGemini` | The LLM (Gemini 2.0 Flash) powering the summary generation |
| **Markdown** | `markdown` | Converts the AI's Markdown output into HTML for email rendering |
| **Gmail** | `gmail` | Sends the finished "Feedback Summary" email |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud) with a publicly reachable webhook URL
- A **Tally** form with three feedback questions, configured to POST submissions to the `/Salah-concert` webhook
- **Google Sheets** OAuth2 credentials + a sheet with columns: Full Name, Email, Phone Number, What went great?, How can we improve?, What is the chance of recommending our event?
- **Google Gemini (PaLM) API** credentials
- A **Gmail** account with OAuth2 credentials connected in n8n

> ⚠️ **Privacy note:** The **Gmail** node has a specific recipient address hardcoded (`khaledmido82@gmail.com`). Before publishing this to a public GitHub repo, consider replacing it with a placeholder (e.g. `YOUR_EMAIL@example.com`) so your personal email isn't exposed in the workflow file.

---

## 🚀 Setup

1. Import `Summarize_Tally_Form.json` into your n8n instance.
2. Activate the workflow and copy the **Post Tally Form** webhook URL.
3. In your Tally form's webhook integration settings, point it to that URL, matching the field order the workflow expects (name, email, phone, then the three feedback questions).
4. Configure Google Sheets, Google Gemini, and Gmail credentials.
5. Update the **Send Form to Google Sheet** / **Get Raw from Google Sheet** nodes to point at your own spreadsheet.
6. Replace the hardcoded recipient in the **Gmail** node with your own email address (see privacy note above).
7. Customize the event name/branding in the LLM prompt if desired.

---

## 🛠️ Customization Ideas

- Batch this to run on a schedule (e.g., weekly) and summarize all feedback collected since the last run, instead of one-by-one per submission
- Send the summary to a Slack channel in addition to (or instead of) email
- Add sentiment scoring or a numeric satisfaction trend chart alongside the written summary
- Support multiple events by parameterizing the event name and routing feedback to event-specific sheets

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
