# 📧 AI Chatbot Email Writer & Sender

An **n8n automation workflow** that turns a Telegram message into a professionally written and delivered email — powered by AI.

---

## 🚀 Overview

This workflow connects **Telegram**, **OpenRouter (GPT-4o-mini)**, and **Gmail** to let users send professional emails simply by typing a recipient address and a brief intent into a Telegram chat. The AI handles writing the full email.

**Example usage:**
```
john@example.com Please remind him about tomorrow's meeting at 10am
```
The bot will write a complete, formal email and send it automatically. ✅

---

## 🔁 Workflow Diagram

```
Telegram Message
      ↓
Validate Input (check for email address)
      ↓
  ┌──────────────────────────┐
  │ Valid?                   │
  Yes                        No
  ↓                          ↓
AI Email Writer        Error: No Email Found
(GPT-4o-mini via         (Telegram reply)
 OpenRouter)
  ↓
Extract Email Parts
(to / subject / body)
  ↓
Send via Gmail
  ↓
  ┌──────────────────────────┐
  │ Success?                 │
  Yes                        No
  ↓                          ↓
✅ Success Reply        ⚠️ Error Reply
  (Telegram)              (Telegram)
```

---

## 🧩 Nodes

| Node | Type | Description |
|------|------|-------------|
| **Email Sender** | Telegram Trigger | Listens for incoming Telegram messages |
| **Validate Input** | Code | Checks message is non-empty and contains an email address |
| **Has Valid Email?** | IF | Routes valid vs. invalid messages |
| **Basic LLM Chain** | LangChain LLM | Sends message to AI for email generation |
| **OpenRouter Chat Model** | OpenRouter | GPT-4o-mini model via OpenRouter API |
| **Code - Extract Email Parts** | Code | Parses AI JSON response into `to`, `subject`, `body` |
| **Gmail - Send Email** | Gmail | Sends the composed email via Gmail OAuth |
| **Success - Email Sent** | Telegram | Notifies user on successful send |
| **Error - No Email Found** | Telegram | Notifies user if no email address was found in message |
| **Error - Send Failed** | Telegram | Notifies user if Gmail send fails |

---

## ⚙️ Setup & Requirements

### Prerequisites

- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- A **Telegram Bot** (via [@BotFather](https://t.me/botfather))
- An **OpenRouter** account with API key
- A **Gmail** account connected via OAuth2

### Credentials Needed

| Service | Credential Type |
|---------|----------------|
| Telegram | Telegram API (Bot Token) |
| OpenRouter | OpenRouter API Key |
| Gmail | Gmail OAuth2 |

### Installation

1. Download the workflow JSON file: `AI_Chatbot_Email_Writer___Sender.json`
2. In n8n, go to **Workflows → Import from File**
3. Upload the JSON file
4. Configure credentials for Telegram, OpenRouter, and Gmail
5. Activate the workflow ✅

---

## 💬 How to Use


1. Open your Telegram bot
2. Send a message in this format:

```
recipient@email.com Your message or intent here
```

3. The bot will:
   - ✍️ Write a full professional email using AI
   - 📤 Send it via your Gmail account
   - ✅ Confirm success (or report an error)

---

## 🛡️ Error Handling

| Scenario | Bot Response |
|----------|-------------|
| Empty message | Usage instructions sent |
| No email address found | Usage instructions sent |
| AI returns invalid JSON | Workflow throws internal error |
| Gmail send fails | Error message sent to user via Telegram |

---

## 🤖 AI Prompt Behavior

The AI is instructed to:
- Extract the recipient email from the start of the message
- Write a **formal, polished professional email**
- Use a proper greeting, structured body, and professional closing
- Sign off as **Mohamed Ibrahim** (customizable in the workflow)
- Return output as raw JSON: `{ "to": "...", "subject": "...", "body": "..." }`

---

## 📁 File Structure

```
├── AI_Chatbot_Email_Writer___Sender.json   # n8n workflow file
└── README.md                               # This file
```

---

## 🧑‍💻 Author

**Mohamed Ibrahim**

---

## 📄 License

This project is open for personal and educational use. Feel free to fork and adapt.
