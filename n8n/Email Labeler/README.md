# 📧 Email Labeling AI Agent

An **n8n workflow** that watches a Gmail inbox in real time and automatically classifies and labels incoming emails using **Google Gemini**, so your inbox organizes itself without manual sorting.

---

## 📋 Overview

Every minute, the workflow checks for new Gmail messages. Each new email's text is passed to an AI text classifier (powered by Gemini 2.0 Flash), which sorts it into one of several predefined categories. Based on the category, the workflow automatically applies the matching Gmail label — no manual triaging required.

**Current categories:**
| Category | Applied when... |
|---|---|
| **ALX VA** | The email asks about a virtual assistant course |
| **Personal** | The sender wants to connect with the manager |
| **Note** | The email contains the word "note" |

---

## 🏗️ Architecture

```
Gmail Trigger (polls every minute)
        │
        ▼
Text Classifier (Gemini 2.0 Flash)
        │
        ├── "ALX VA"    → Gmail: Add Label (ALX VA)
        ├── "Personal"  → Gmail: Add Label (Personal)
        └── "Note"      → Gmail: Add Label (Notes)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **Gmail Trigger** | `gmailTrigger` | Polls the inbox every minute for new messages |
| **Text Classifier** | `textClassifier` | Classifies the email text into one of the defined categories using an LLM |
| **Google Gemini Chat Model** | `lmChatGoogleGemini` | The language model (Gemini 2.0 Flash) powering the classifier |
| **ALX VA** | `gmail` | Applies the "ALX VA" label to matching emails |
| **Personal** | `gmail` | Applies the "Personal" label to matching emails |
| **Notes** | `gmail` | Applies the "Notes" label to matching emails |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud)
- A **Gmail** account with OAuth2 credentials connected in n8n
- **Google Gemini (PaLM) API** credentials
- The target Gmail **labels already created** in your inbox (the workflow only adds existing label IDs, it does not create new labels)

---

## 🚀 Setup

1. Import `Email_Labeling_AI_Agent.json` into your n8n instance.
2. Connect your **Gmail** OAuth2 credentials and **Google Gemini** API credentials.
3. In Gmail, create the labels you want to use (or reuse existing ones) and note their label IDs.
4. Update each Gmail node (`ALX VA`, `Personal`, `Notes`) with the correct `labelIds` for your own inbox — the current IDs (`Label_3504084860964995072`, `Label_3`, `Label_5`) are specific to the original account and won't match yours.
5. Edit the categories and descriptions inside the **Text Classifier** node to match how you want your own emails sorted.
6. Activate the workflow.

---

## 🛠️ Customization Ideas

- Add more categories (e.g., "Invoices," "Spam-review," "Urgent") with their own labels
- Chain in an auto-reply or auto-archive step for specific categories
- Add a fallback/"Uncategorized" branch for emails that don't clearly match any category
- Extend the classifier to also flag priority or sentiment alongside the category

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
