# 🛠️ Setup Guide

This guide walks you through setting up the Smart WhatsApp AI Agent from scratch.

---

## Step 1 — Install n8n

**Option A: Cloud (easiest)**
Sign up at [app.n8n.cloud](https://app.n8n.cloud) — free tier available.

**Option B: Self-hosted with Docker**
```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```
Then visit `http://localhost:5678`.

---

## Step 2 — Set Up Your WhatsApp API

You need a WhatsApp Business API provider. Two options:

**Option A: Meta Cloud API (free)**
1. Go to [developers.facebook.com](https://developers.facebook.com/)
2. Create an App → Add WhatsApp product
3. Get your **Phone Number ID** and **Access Token**
4. Set your webhook URL to the n8n webhook URL from Workflow 01

**Option B: 360dialog**
1. Sign up at [360dialog.com](https://www.360dialog.com/)
2. Connect your WhatsApp number
3. Get your API key and set the webhook URL

---

## Step 3 — Create API Accounts

### OpenAI
1. Go to [platform.openai.com](https://platform.openai.com/)
2. Create an API key → copy to `OPENAI_API_KEY`

### Zep Memory
1. Go to [getzep.com](https://www.getzep.com/) → sign up
2. Create a project → copy API key to `ZEP_API_KEY`

### Pinecone
1. Go to [pinecone.io](https://www.pinecone.io/) → sign up
2. Create an index with:
   - **Dimensions:** `1024` (for Cohere embed-english-v3.0)
   - **Metric:** `cosine`
3. Copy your API key and index name to `.env`

### Cohere
1. Go to [dashboard.cohere.com](https://dashboard.cohere.com/) → sign up
2. Copy your API key to `COHERE_API_KEY`

### Google (Gmail + Calendar)
1. Go to [console.cloud.google.com](https://console.cloud.google.com/)
2. Create a project → Enable **Gmail API** and **Google Calendar API**
3. Create OAuth 2.0 credentials (Desktop App)
4. Copy Client ID and Secret to `.env`
5. In n8n, add a Google OAuth2 credential and authenticate

### X.com (Twitter) API
1. Go to [developer.twitter.com](https://developer.twitter.com/)
2. Create a project and app
3. Generate API keys and access tokens → copy to `.env`

---

## Step 4 — Import Workflows into n8n

Import the workflows **in order**:

1. In n8n, go to **Workflows → Import from File**
2. Import `workflows/01_data_input_transformation.json`
3. Import `workflows/02_ai_agent.json`
4. Import `workflows/03_rag.json`
5. Import `workflows/04_communication.json`

---

## Step 5 — Configure Credentials in n8n

For each workflow, open the nodes and attach the correct credentials:

| Node | Credential Type |
|------|----------------|
| Webhook | (none — auto-configured) |
| AI Agent | OpenAI API |
| Zep Memory v3 | Zep API |
| Pinecone Vector Store | Pinecone API |
| Cohere Embeddings | Cohere API |
| Gmail nodes | Google OAuth2 |
| Calendar nodes | Google OAuth2 |
| X.com nodes | Twitter OAuth1 |

---

## Step 6 — Load Your Knowledge Base (RAG)

1. Open Workflow 03 (RAG)
2. Connect your Google Drive folder containing the documents you want the agent to know about
3. Run the workflow once to embed and index all documents into Pinecone
4. The agent can now answer questions from those documents

---

## Step 7 — Activate and Test

1. **Activate** all four workflows in n8n (toggle to ON)
2. Send a WhatsApp message to your connected number
3. Check the n8n execution logs to see the flow in action

---

## 🐛 Troubleshooting

**Webhook not receiving messages**
- Make sure your n8n instance is publicly accessible (use [ngrok](https://ngrok.com/) for local testing)
- Double-check the webhook URL is correctly set in your WhatsApp provider

**Agent not remembering past conversations**
- Verify your Zep API key is correct
- Check the Zep Memory node is connected to the Agent node

**RAG returning no results**
- Make sure Workflow 03 was run at least once to populate Pinecone
- Check that the Pinecone index name matches your `.env`

**Voice messages not transcribing**
- Confirm the OpenAI Whisper (or equivalent) node has a valid API key
