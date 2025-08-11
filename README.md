# 🤖 GenAI Personal Assistant (n8n + WhatsApp + OpenAI)

## **The information below is generated using an LLM**

This project builds a fully automated **personal assistant** powered by **n8n**, **OpenAI**, and **Twilio WhatsApp**, capable of answering natural language queries and routing them to purpose-built agents.

It requires no external UI—everything runs through WhatsApp and n8n’s workflow engine.

---

## 🔍 Features

✅ WhatsApp-based natural language interface  
✅ Intelligent tool-calling agent using GPT-4o-mini  
✅ Fine-tuned classification model for email alerts  
✅ Semantic search over stored emails (PGVector)  
✅ Google Calendar integration (view/add/edit/delete events)  
✅ Modular architecture with reusable agent workflows  
✅ Optional browser-based RAG (via Tavily) fallback

---

## 🛠️ Architecture

```
          ┌────────────────────────┐
          │ WhatsApp Message (User)│
          └────────────┬───────────┘
                       ▼
             [ Twilio Webhook → n8n ]
                       ▼
          ┌────────────────────────────┐
          │ Personal Assistant Agent   │
          │ (LLM + tool selector)      │
          └────────────────────────────┘
             │     │         │     │
             ▼     ▼         ▼     ▼
      Email Agent  Calendar  Tavily  Weather
         (RAG)       API       API     API
```

---

### 📬 Email Ingestion Workflow

Incoming emails are automatically ingested and passed through a pipeline that:

- Embeds and stores their content in PGVector
- Classifies each email using a **fine-tuned GPT-4o-mini**
- Summarizes the message using **GPT-4o-mini** (base)
- Stores metadata (date, classification, messageId) for future RAG use

This allows the assistant to retrieve and reason over real emails without polling Gmail in real-time.

---

## 💬 Demo Workflow

1. You send a WhatsApp message like:
   ```
   What's on my calendar next week?
   ```
2. n8n receives the message via Twilio webhook
3. Personal Assistant Agent routes the message to:
   - `Calendar Agent`, which calls Google Calendar and returns events
4. Response is sent back to WhatsApp automatically

Try:
- “Did I get any internship rejections last month?”
- “Add meeting with David Friday at 3pm”
- “Weather in New York tomorrow”

---

## 🧠 Fine-Tuning Procedure

The assistant uses a **fine-tuned GPT-4o-mini** model to classify email messages.  
See `notebooks/` for:

- `model-fine-tuning.ipynb`: Parses & formats Gmail text for fine-tuning
- `fine-tune-performance-test.ipynb`: Evaluations of the fine-tuning process

Classification labels:
- `Internship`: status updates, interview invites, rejections
- `Canvas`: grade releases and course announcements
- `Personal`: handwritten messages, non-promotional
- `Irrelevant`: everything else

Actual fine-tuning through the OpenAI dashboard.

---

## 🔐 Credentials Needed

Store credentials via n8n UI:
- Twilio WhatsApp
- Google Gmail OAuth
- Google Calendar OAuth
- OpenAI API Key
- PostgreSQL (with pgvector)

---

## 🚀 Setup & Run

1. Install [n8n](https://docs.n8n.io/)
2. Import workflows from `workflows/`
3. Set credentials under **Settings > Credentials**
4. Start server (e.g. `n8n start` or cloud deploy)
5. Send WhatsApp message to your Twilio number

---

## 👤 Author

Created by **Kevin Zheng** for Red Ventures Data Science Product Manager Internship (2025).

---

## 📄 License

MIT License
