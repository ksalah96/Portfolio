# Restaurant AI Concierge

An n8n workflow that turns Telegram into a full-service restaurant assistant — an AI-powered ordering concierge, automatic client logging, post-visit review requests, and a daily marketing broadcast system, all driven from a single configuration panel.

## Overview

This workflow lets customers chat (by text or voice note) with an AI concierge on Telegram to browse the menu, build an order, and receive a smart upsell suggestion — all in natural conversation. Every customer who messages the bot is automatically logged, sent a review request a few hours later, and added to a daily marketing broadcast list. Restaurant owners only need to edit one node to update branding, links, and behavior.

## Features

- **Conversational ordering** — customers ask about the menu, get accurate answers pulled live from a Google Sheet, and place an order without a rigid button-based flow.
- **Text + voice support** — voice notes are automatically transcribed with OpenAI Whisper before being handled by the same AI agent as text messages.
- **Smart upselling** — the agent proposes one relevant add-on per order (drink, side, or dessert) based on a configurable instruction, without being pushy about it.
- **Order logging** — confirmed orders (items, total, upsell outcome) are saved to a Google Sheet automatically.
- **Automatic client capture** — every customer who messages the bot is logged with name, ID, and first-contact date.
- **Review requests** — a configurable delay after first contact, customers receive a friendly request to leave a Google review.
- **Daily marketing broadcasts** — a scheduled job reads that day's campaign message from a Marketing sheet and sends it to every active (non-unsubscribed) client, then marks the campaign as sent.
- **Single point of configuration** — restaurant name, menu link, review link, AI model, upsell instruction, and review delay are all set in one node.

## How It Works

**1. Configuration**
All editable settings live in one node: restaurant name, menu/review links, OpenAI model, upsell instruction, and review delay (in hours). This feeds both the concierge chat and the marketing broadcast.

**2. AI Concierge Chat**
Incoming Telegram messages are routed by type (text, voice, or unsupported). Voice notes are transcribed with OpenAI Whisper and normalized into the same format as text. Both feed into an AI agent that:
- Looks up menu items and prices via a Google Sheets tool (never invents dishes or prices)
- Builds the order conversationally, confirming items and quantities
- Proposes one upsell per order, per the configured instruction
- Saves the finalized order to a Google Sheet once the customer confirms
- Replies to the customer on Telegram in the same language they used

**3. Client Logging and Review Request**
Every incoming message logs (or updates) the customer in a Clients sheet. The workflow then waits for a configurable number of hours before sending a review request with a link to leave a Google review.

**4. Daily Marketing Broadcast**
On a daily schedule, the workflow checks the Marketing sheet for that day's campaign message, fetches the list of active (non-unsubscribed) clients, sends the message to each one, and marks the campaign as done so it isn't sent twice.

## Requirements

- **n8n** (self-hosted or cloud)
- **Telegram Bot** — created via [@BotFather](https://t.me/BotFather), connected via a Telegram Trigger/webhook
- **OpenAI API** — for the chat model (agent) and Whisper transcription (voice notes)
- **Google Sheets** — a spreadsheet with the following tabs:
  - `Menu` — item names, descriptions, prices
  - `Orders` — order log (populated automatically)
  - `Clients` — customer log (populated automatically), with an `UNSUBSCRIBE` column
  - `Marketing` — scheduled campaign messages, with `DATE`, `MESSAGE`, and `DONE` columns

## Setup

1. Import the workflow JSON into n8n.
2. Create a Telegram bot and connect its credentials to the Telegram nodes.
3. Connect your OpenAI credentials to the Chat Model and Whisper transcription nodes.
4. Connect your Google Sheets credentials and point each Google Sheets node to your spreadsheet (Menu, Orders, Clients, Marketing tabs).
5. Open the **Configuration** node and set:
   - `resturant_name` — your restaurant's name
   - `menu_link` — link to your digital menu
   - `review_link` — your Google review link
   - `openai_model` — the OpenAI model to use for the agent
   - `upsell_instruction` — how the agent should upsell
   - `review_wait_hours` — delay before sending the review request
6. Set the **Daily Marketing Schedule** trigger to your preferred send time.
7. Activate the workflow.

## Notes

- Menu prices and items are always fetched live from the `Menu` sheet — nothing is hardcoded in the agent's prompt, so menu updates take effect immediately without editing the workflow.
- The `Orders` and `Clients` sheets populate automatically; no manual data entry is required after setup.
- Before publishing your own copy of this workflow, replace any placeholder values (restaurant name, menu link, review link) with your own, and double-check no real customer data or credentials are pinned to any node.

## Disclaimer

This workflow is provided as a template for educational and portfolio purposes. Replace all placeholder links and credentials with your own before deploying in production.

---

**Author:** Khaled Salah
