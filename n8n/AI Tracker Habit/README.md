# ✅ AI Habit Tracker

An **n8n automation workflow** that sends a daily Telegram check-in reminder, uses **Google Gemini 2.5 Flash** to turn your habit reply into a short motivational message, and logs the entry into a **Notion** database — building a running, encouraging record of your daily habits.

---

## 📋 Overview

Every day at a scheduled time, the workflow pings you on Telegram asking what habit you completed. When you reply, an AI agent reads your message, generates a brief motivational response, and the whole exchange (habit + date + motivation) gets saved to a Notion database. You then get a confirmation reply on Telegram.

**Flow at a glance:**
1. ⏰ Daily reminder sent to Telegram
2. 💬 User replies with their completed habit
3. 🤖 AI Agent generates a 1–2 sentence motivational message
4. 📝 Habit, date, and motivation are logged to Notion
5. ✅ Confirmation sent back to the user on Telegram

---

## 🏗️ Architecture

```
Schedule Trigger ──► Sent to User (Telegram reminder)

Receive Habit (Telegram reply) ──► AI Agent (Gemini 2.5 Flash + Postgres memory)
                                          │
                                          ▼
                                 Extract Field to Notion
                                          │
                                          ▼
                                       Notion
                                          │
                                          ▼
                                  Reply to User (Telegram)
```

### Node Breakdown

| Node | Type | Purpose |
|---|---|---|
| **Schedule Trigger** | `scheduleTrigger` | Fires daily at a set time (default: 9:53 AM) to send the check-in reminder |
| **Sent to User** | `telegram` | Sends the daily "time for your habit check-in" message |
| **Receive Habit** | `telegramTrigger` | Listens for the user's reply message |
| **AI Agent** | `agent` | Generates a short, uplifting motivational reply based on the habit shared (powered by Gemini) |
| **Google Gemini Chat Model** | `lmChatGoogleGemini` | The LLM (Gemini 2.5 Flash) powering the AI Agent |
| **Postgres Chat Memory** | `memoryPostgresChat` | Keeps per-user conversation history, keyed by Telegram chat ID |
| **Extract Field to Notion** | `Set` | Structures the `Habit`, `Date`, and `Motivation` fields for Notion |
| **Notion** | `notion` | Creates a new page/entry in a Notion "Habit Tracker" database |
| **Reply to User** | `telegram` | Sends a confirmation message back to the user with the motivational text |

---

## 🔑 Prerequisites

- An **n8n** instance (self-hosted or cloud)
- A **Telegram Bot** (via [BotFather](https://t.me/BotFather)) and its API credentials
- **Google Gemini (PaLM) API** credentials
- A **Postgres** database (for per-user chat memory)
- A **Notion** integration with access to a "Habit Tracker" database containing `Habit` (title), `Date` (date), and `Motivation` (rich text) properties

> ⚠️ **Privacy note:** The workflow JSON has a **specific Telegram chat ID hardcoded** in the "Sent to User" node (`chatId: "2004322048"`). Before publishing this to a public GitHub repo, replace it with a placeholder (e.g. `YOUR_TELEGRAM_CHAT_ID`) or move it into an n8n environment variable / credential so your personal chat ID isn't exposed.

---

## 🚀 Setup

1. Import `AI_Habit_Tracker.json` into your n8n instance.
2. Configure credentials for Telegram, Google Gemini, Postgres, and Notion.
3. Replace the hardcoded `chatId` in the **Sent to User** node with your own Telegram chat ID (or an environment variable — see privacy note above).
4. Point the **Notion** node at your own "Habit Tracker" database, matching the `Habit`, `Date`, and `Motivation` properties.
5. Adjust the **Schedule Trigger** to your preferred daily check-in time.
6. Activate the workflow.

---

## 🛠️ Customization Ideas

- Support multiple users by looking up each user's Notion database/chat ID dynamically instead of hardcoding one
- Add a weekly/monthly summary agent that reviews Notion entries and reports on streaks or consistency
- Add habit categories or tagging so Notion entries can be filtered by type (fitness, learning, etc.)
- Swap Telegram for WhatsApp Business API for the check-in and reply flow

---

## 📄 License

Feel free to use and adapt this workflow for your own projects.

---

**Author:** Khaled Salah
