# 🤖 Smart WhatsApp AI Agent

An intelligent, no-code WhatsApp automation agent built with **n8n** that can understand text, voice, and image messages — and respond using AI, your calendar, Gmail, and more.

> Full workflow available in the original LinkedIn post by [@IbrahimG](https://www.linkedin.com/in/)

---

## ✨ Features

- 💬 **Multi-modal input** — handles text, voice notes, and images from WhatsApp
- 🧠 **AI-powered responses** — uses GPT-o3 as the language brain
- 🗂️ **Long-term memory** — remembers past conversations via Zep Memory v3
- 📚 **RAG (Knowledge Base)** — answers questions from your private documents using Pinecone + Cohere
- 📅 **Calendar integration** — checks, creates, and manages Google Calendar events
- 📧 **Gmail integration** — reads, summarizes, and drafts emails
- 🐦 **X.com (Twitter)** — reads and publishes posts on your behalf
- 🖼️ **Image analysis** — uses AI vision to understand and describe images (OCR, object detection)
- 🎙️ **Voice transcription** — converts audio messages into text automatically

---

## 🏗️ Architecture

The system is split into **4 n8n sub-workflows**:

```
WhatsApp Message
      │
      ▼
┌─────────────────────────────┐
│  1. Data Input & Transform  │  ← Routes Text / Voice / Image
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│       2. AI Agent           │  ← GPT-o3 + Zep Memory + Tools
└─────────────┬───────────────┘
         ┌────┴────┐
         ▼         ▼
┌──────────────┐  ┌──────────────────┐
│   3. RAG     │  │ 4. Communication │
│  (Pinecone)  │  │ Gmail/Cal/X.com  │
└──────────────┘  └──────────────────┘
```

### Workflow Breakdown

| # | Workflow | Description |
|---|----------|-------------|
| 1 | **Data Input & Transformation** | Webhook trigger → routes messages by type (text/voice/image) → converts and analyzes media |
| 2 | **AI Agent** | Central brain (GPT-o3) with memory, RAG access, and tool connections |
| 3 | **RAG** | Loads documents into Pinecone vector store via Cohere embeddings for smart retrieval |
| 4 | **Communication** | MCP server connections to Gmail, X.com, and Google Calendar |

---

## 📁 Repository Structure

```
smart-whatsapp-ai-agent/
├── README.md
├── LICENSE
├── .env.example                         # All required environment variables
├── workflows/
│   ├── 01_data_input_transformation.json
│   ├── 02_ai_agent.json
│   ├── 03_rag.json
│   └── 04_communication.json
├── docs/
│   ├── setup.md                         # Step-by-step setup guide
│   ├── architecture.md                  # Detailed architecture notes
│   └── screenshots/                     # Workflow screenshots
└── assets/
    └── smart-whatsapp-ai-agent.pdf      # Original overview PDF
```

---

## 🚀 Getting Started

### Prerequisites

- [n8n](https://n8n.io/) (self-hosted or cloud) — free & open source
- A WhatsApp Business API provider (e.g. [360dialog](https://www.360dialog.com/), [Meta Cloud API](https://developers.facebook.com/docs/whatsapp))
- API keys for the services below

### Required Services

| Service | Purpose | Free Tier? |
|---------|---------|------------|
| OpenAI (GPT-o3) | Language model | ✅ Limited |
| [Zep](https://www.getzep.com/) | Long-term memory | ✅ Yes |
| [Pinecone](https://www.pinecone.io/) | Vector database for RAG | ✅ Yes |
| [Cohere](https://cohere.com/) | Text embeddings | ✅ Yes |
| Google (Gmail + Calendar) | Email & scheduling | ✅ Yes |
| X.com (Twitter) API | Post/read tweets | ✅ Limited |

### Installation

1. **Clone this repo**
   ```bash
   git clone https://github.com/YOUR_USERNAME/smart-whatsapp-ai-agent.git
   cd smart-whatsapp-ai-agent
   ```

2. **Copy the environment file**
   ```bash
   cp .env.example .env
   ```

3. **Fill in your credentials** in `.env` (see `.env.example` for all keys)

4. **Import workflows into n8n**
   - Open n8n → Settings → Import Workflow
   - Import each JSON file from the `/workflows` folder in order (01 → 04)

5. **Configure your WhatsApp webhook**
   - Copy the webhook URL from the `Webhook` node in workflow `01`
   - Paste it into your WhatsApp API provider as the inbound message URL

6. **Activate all workflows** in n8n

---

## 📖 Detailed Setup

See [`docs/setup.md`](docs/setup.md) for a full step-by-step guide including:
- Setting up each credential in n8n
- Configuring Pinecone indexes
- Loading documents into the RAG knowledge base
- Testing your WhatsApp connection

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙌 Credits

Built and designed by **IbrahimG** — follow on LinkedIn for more AI automation workflows.
