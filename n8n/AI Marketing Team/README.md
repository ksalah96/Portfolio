# 🧠 AI Marketing Team

A multi-agent **n8n workflow** that acts as an autonomous marketing department. A **CMO AI Agent** receives requests via chat, delegates them to specialist AI agents (Content & Email, Paid Ads, Social Media), reviews their research-backed deliverables, and logs everything into a Google Sheet — all through natural conversation.

---

## 📋 Overview

Instead of manually briefing different specialists for every marketing task, this workflow lets you simply **chat your request**. A CMO Agent interprets the request, asks clarifying questions if needed, assigns it to the right specialist agent, and makes sure the final deliverable is research-backed, reviewed, and logged for tracking.

**Example:**
> "Create a TikTok ad campaign for our new coffee brand targeting Gen Z"

→ CMO Agent confirms budget/goal/duration → routes to the Paid Ads Agent → agent researches trends via Tavily → returns a full campaign plan → CMO reviews and logs it to Google Sheets.

---

## 🏗️ Architecture

```
Chat Trigger
     │
     ▼
AI CMO Agent (orchestrator)
     │
     ├── Think (reasoning/scratchpad tool)
     ├── CMO Log (Google Sheets — logs every completed task)
     ├── Postgres Chat Memory (conversation history)
     │
     ├── AI Content + Email Marketing Agent ── Tavily Content Research
     ├── AI Paid Ads Agent ────────────────── Tavily Ads Research
     └── AI Social Media Agent ─────────────── Tavily Social Media Research
```

Each specialist agent runs its own Google Gemini chat model and has its own Tavily-powered research tool, so it can pull real, current information before producing a deliverable.

---

## 🤖 Agents

### CMO AI Agent (Orchestrator)
The entry point for every request. It:
- Clarifies missing details (audience, budget, timeline, platforms) before delegating
- Routes tasks to the correct specialist agent
- Reviews returned deliverables for completeness and alignment
- Logs every task into a structured **CMO Log** Google Sheet (23 fields, including KPIs, audience insights, competitor insights, recommendations, status, and next steps)
- Uses a **Think** tool to double-check reasoning before finalizing a review

### AI Content + Email Marketing Agent
Handles newsletters, promotional emails, blog posts, PR copy, and email automation plans. Researches via Tavily before writing, and always delivers plain-text drafts back to the CMO for approval (no direct sending).

### AI Paid Ads Agent
Builds and optimizes paid campaigns for Meta, TikTok, and Google Ads — targeting, budget/bidding strategy, creative concepts, and A/B testing plans, backed by competitor and keyword research via Tavily.

### AI Social Media Agent
Builds organic social strategies: content calendars, trending formats, hashtags, hooks/captions, posting schedules, and influencer collaboration ideas, researched via Tavily.

---

## 🔌 Nodes & Integrations

| Component | Type | Purpose |
|---|---|---|
| **When chat message received** | `chatTrigger` | Entry point for conversational requests |
| **AI CMO Agent** | `agent` | Orchestrates delegation, review, and logging |
| **Think** | `toolThink` | Scratchpad/reasoning tool for the CMO agent |
| **CMO Log** | `googleSheetsTool` | Appends completed task records to a Google Sheet |
| **Postgres Chat Memory** | `memoryPostgresChat` | Persists conversation history across sessions |
| **AI Content + Email Marketing Agent** | `agentTool` | Specialist sub-agent for content/email |
| **AI Paid Ads Agent** | `agentTool` | Specialist sub-agent for paid campaigns |
| **AI Social Media Agent** | `agentTool` | Specialist sub-agent for organic social |
| **Tavily Content/Ads/Social Research** | `httpRequestTool` | Research calls to the Tavily Search API for each specialist agent |
| **Google Gemini Chat Model(s)** | `lmChatGoogleGemini` | Language model powering the CMO and each specialist agent |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud) with LangChain nodes enabled
- **Google Gemini (PaLM) API** credentials
- **Google Sheets** OAuth2 credentials + a sheet formatted with the CMO Log columns (Date, Task ID, Task Name, Agent Assigned, Campaign Objective, KPIs, Target Audience, etc.)
- **Postgres** database credentials (for chat memory)
- A **Tavily** API key for web research

> ⚠️ **Security note:** The current workflow JSON has a **live Tavily API key hardcoded** in the Authorization header of all three research nodes (Tavily Content/Ads/Social Research). Before pushing this file to a public GitHub repo, replace the key with a placeholder (e.g. `Bearer {{$credentials.tavilyApi.apiKey}}` or move it into n8n credentials) and rotate the exposed key — otherwise anyone who reads the file can use your Tavily quota.

---

## 🚀 Setup

1. Import `AI_Marketing_Team.json` into your n8n instance.
2. Set up credentials for Gemini, Google Sheets, Postgres, and Tavily.
3. Update the **CMO Log** node to point to your own Google Sheet, matching the 23-column schema used in the workflow.
4. Move the Tavily API key out of the HTTP Request nodes and into n8n's credential store (see security note above).
5. Activate the workflow and open the chat trigger to start delegating marketing tasks.

---

## 🛠️ Customization Ideas

- Add more specialist agents (e.g., SEO, Influencer Outreach, PR)
- Add a human-approval step before the CMO logs a task as "Approved"
- Connect the Content Agent's drafts directly to an email platform (Mailchimp, Klaviyo, etc.) once reviewed
- Extend the Paid Ads Agent with direct API connections to Meta/Google Ads for launch automation

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
