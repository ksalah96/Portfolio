# 🤖 AI Content Distributor

An **n8n automation workflow** that monitors an RSS feed, uses **Google Gemini 2.5 Flash** to rewrite blog posts into platform-optimized social media content, and automatically distributes it to **X (Twitter), LinkedIn, and Slack**.

---

## 📋 Overview

This workflow eliminates the manual work of turning a blog post into social media content. Once a new article appears in an RSS feed, the workflow:

1. Pulls the latest post (title, summary, link)
2. Cleans and structures the data
3. Sends it to an AI agent (Gemini 2.5 Flash) that rewrites it into a Twitter/X post and a LinkedIn post
4. Parses the AI output into separate fields
5. Publishes the content to X, LinkedIn, and a Slack channel — automatically, every hour

---

## ⚙️ How It Works

```
RSS Feed Trigger  →  Clean Input for AI  →  Basic LLM Chain (Gemini 2.5 Flash)  →  Extract Content  →  ┬─ X (Twitter)
                                                                                                          ├─ LinkedIn
                                                                                                          └─ Slack
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **RSS Feed Trigger** | `rssFeedReadTrigger` | Polls the RSS feed every hour for new blog posts |
| **Clean Input for AI** | `Set` | Extracts and normalizes `Title`, `Link`, and `Content` from the raw RSS item |
| **Basic LLM Chain** | `chainLlm` | Sends a prompt to the AI model to generate a Twitter/X post (≤280 chars) and a LinkedIn post (~600 chars), each with a CTA and link |
| **Generate Social Text** | `lmChatGoogleGemini` | The language model powering the LLM Chain (Gemini 2.5 Flash) |
| **Extract Content** | `Set` | Uses regex to split the AI's raw text output into separate `Twitter/x` and `LinkedIn` fields |
| **X** | `twitter` | Publishes the Twitter/X post |
| **LinkedIn** | `linkedIn` | Publishes the LinkedIn post to an organization page |
| **Slack** | `slack` | Posts the Twitter/X version into a Slack channel for visibility/review |

---

## 🔑 Prerequisites

To run this workflow, you'll need:

- An **n8n** instance (self-hosted or cloud)
- An **RSS feed URL** to monitor
- API credentials for:
  - **Google Gemini (PaLM) API** — for content generation
  - **X (Twitter) OAuth2** — for posting tweets
  - **LinkedIn API** — for posting to an organization page
  - **Slack API** — for posting to a channel

---

## 🚀 Setup

1. Import `AI_Content_Distributor.json` into your n8n instance.
2. Replace the **RSS Feed Trigger** URL with your own feed.
3. Configure credentials for Gemini, X, LinkedIn, and Slack in their respective nodes.
4. Update the **Slack** node's channel ID to your target channel.
5. Update the **LinkedIn** node's organization ID to your own page.
6. Activate the workflow.

---

## 🛠️ Customization Ideas

- Add **Facebook / Instagram** nodes to extend distribution further
- Add a **human-in-the-loop approval step** (e.g., Slack approval buttons) before auto-posting
- Swap Gemini for another LLM provider (OpenAI, Claude, etc.)
- Adjust the prompt to change tone, add hashtags, or target additional platforms

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
