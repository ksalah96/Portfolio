# AI Research Agent (n8n Workflow)

An autonomous research agent built in **n8n** using the LangChain Agent node. Give it a topic in chat, and it searches the web, scrapes relevant articles, and returns a structured research report — citing every source it used.

## How It Works

The agent follows a strict **Observe → Think → Act** loop:

1. **Web Search** – Queries the web to discover relevant URLs and get summary snippets.
2. **Web Scrape** – Pulls full text content from the article URLs returned by search (never scrapes blindly, never scrapes social media, PDFs, or login pages).
3. **Report Generation** – After 2 searches and 2 successful scrapes, it stops looping and writes the final report.

The system prompt enforces hard rules: no fabricated facts, no guessing URLs, no scraping before searching, and no duplicate scrapes. If a scrape fails or a search comes up empty, the agent adapts gracefully instead of stalling.

### Output Format

Every report follows the same structure:

```
# Research Report: [Topic]
## Executive Summary
## Key Findings
## Detailed Analysis
## Sources
## Next Steps
```

## Workflow Architecture

| Node | Role |
|---|---|
| **Chat Trigger** | Entry point — starts the workflow from a chat message |
| **AI Agent** | Core LangChain agent orchestrating the research loop |
| **Gemini (LLM)** | Language model powering the agent's reasoning |
| **Simple Memory** | Buffer window memory for conversational context |
| **Web Search** | SearchAPI-powered tool for discovering sources |
| **Web Scrape** | Jina AI-powered tool for extracting full page content |
| **Respond to Chat** | Sends the final report back to the user |

## Setup

### Prerequisites

- An [n8n](https://n8n.io) instance (self-hosted or cloud)
- API credentials for:
  - **Google Gemini** (LLM)
  - **SearchAPI** (web search)
  - **Jina AI** (web scraping)

### Installation

1. Import `AI_Research_Agent.json` into your n8n instance (**Workflows → Import from File**).
2. Configure credentials for each of the three services above under the corresponding nodes.
3. Activate the workflow.
4. Open the chat interface and send a research topic to get started.

## Customization

- **Model**: Swap the Gemini node for any other LangChain-compatible chat model.
- **Loop limits**: Adjust the "2 searches + 2 scrapes" stopping condition in the system prompt to allow deeper research.
- **Output format**: Edit the system prompt's `OUTPUT FORMAT` section to match your preferred report structure.

## License

Feel free to use and adapt this workflow for your own projects.

---

Author: Khaled Salah
