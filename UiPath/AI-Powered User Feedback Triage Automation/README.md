# AI-Powered User Feedback Triage & Categorization (UiPath)

An intelligent Robotic Process Automation (RPA) workflow built with **UiPath Studio** and **UiPath Integration Service** (GenAI Activities). This automation monitors incoming user feedback emails, summarizes and categorizes them using Generative AI, performs semantic similarity checks against existing records, and logs structured insights directly into Microsoft OneDrive Excel spreadsheets.

---

## 🚀 Overview

Manually reading, categorizing, and deduplicating customer feedback emails is time-consuming and prone to inconsistency. This solution automates the end-to-end triage pipeline:

1. **Email Monitoring:** Filters incoming Outlook/Microsoft 365 emails from today matching specific sender and subject criteria (`"You’ve received new feedback"`).
2. **Context-Grounded Summarization:** Uses **Gemini 3.7 Flash** via UiPath GenAI activities—grounded against a reference Word document (`Examples.docx`)—to condense verbose emails into a single, standardized summary sentence.
3. **Intelligent Classification:** Automatically categorizes the summarized feedback into one of five distinct buckets:
   * `Bug report`
   * `Feature request`
   * `Complaint`
   * `Praise`
   * `Other`
4. **Semantic Deduplication & Logging:** 
   * **Bug Reports & Feature Requests:** Compares the new summary against existing rows in `User feedback.xlsx` using **Semantic Similarity**. If the similarity score meets the threshold (`>= 0.2`), it appends the entry to the respective sheet (`Bug report` or `Feature request`).
   * **Complaints, Praises & Other:** Extracts the sender/referenced email addresses from the body using Regex/Text Extraction, cleans the core message body via GenAI, and appends the email address alongside the extracted feedback into the corresponding category sheet.

---

## 🛠️ Tech Stack & Integrations

* **RPA Platform:** UiPath Studio (Windows / Cross-Platform XAML)
* **AI & NLP (UiPath Integration Service):**
  * `Content Generation` (`gemini-3.7-flash` with File Resource Context Grounding)
  * `Categorize` (Multi-class intent classification)
  * `Semantic Similarity` (String-to-string vector comparison)
* **Microsoft 365 Integration:**
  * Outlook / Mail Activities (`For Each Email`)
  * OneDrive / SharePoint File Activities (`Get File or Folder`)
  * Excel Online Activities (`For Each Row in Workbook`, `Write Row`)

---

## 📂 Prerequisites & Setup

Before running this workflow in your own environment, ensure the following connections and cloud files are configured:

### 1. UiPath Integration Service Connections
Configure active connections in **UiPath Orchestrator / Integration Service** for:
* **Microsoft Outlook 365** (Email retrieval)
* **Microsoft OneDrive & SharePoint** (Reading/writing Excel and Word files)
* **UiPath GenAI / Elements Service** (Access to `gemini-3.7-flash` models)

### 2. Cloud Files (OneDrive)
Create the following files in your connected OneDrive root directory (or update the activity file pickers to match your paths):
* **`Examples.docx`**: A reference document containing standard phrasing examples for known issues and feature requests to guide the GenAI context grounding.
* **`User feedback.xlsx`**: An Excel workbook containing the following worksheet tabs:
  * `Bug report` (Headers: `Issue ID`, `Known Issue Description`)
  * `Feature request` (Headers: `Request ID`, `Feature Request Description`)
  * `Complaint`
  * `Praise`
  * `Other`

### 3. Email Filter Customization
In the **For Each Email** activity, update the `From` filter condition (currently set to `k.salah0610@gmail.com`) to match your target feedback inbox or notification sender.

---

## 🔄 Workflow Architecture

```text
[Manual Trigger]
       │
       ▼
[For Each Email (Filtered by Sender, Date = Today, Subject)]
       │
       ├──► Fetch Context File ('Examples.docx' from OneDrive)
       ├──► GenAI: Summarize Email Body (Context-Grounded)
       ├──► GenAI: Categorize Summary
       │
       ├──► IF Category == "Bug report"
       │      └──► Loop 'Bug report' sheet ──► Semantic Similarity Check ──► Append Row
       │
       ├──► ELSE IF Category == "Feature request"
       │      └──► Loop 'Feature request' sheet ──► Semantic Similarity Check ──► Append Row
       │
       └──► ELSE ("Complaint", "Praise", "Other")
              └──► Extract Email Addresses ──► GenAI: Clean Body ──► Append Row to Category Sheet
```

---

## 👨‍💻 Author

**Khaled Salah**