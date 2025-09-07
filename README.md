# 📲 WhatsApp Catering Agent — n8n

Multi‑channel WhatsApp agent built in **n8n** for a catering service in **Salta, Argentina**.  
It supports **text, voice, and image** messages, and uses two tools to respond with accurate, up‑to‑date information:

- **Budgets** → MCP endpoint backed by **Google Docs** (menus, components, service terms, logistics, availability).
- **Prices** → **Google Sheets** (pricing by service type and guest count).

The agent replies **in the same modality** (text/voice/image) and keeps **short per‑contact memory** to avoid repeating questions.

---

## 📌 Executive Summary
This workflow centralizes WhatsApp inquiries for a catering business. It qualifies the request (event type, date, guest count, location), consults **Budgets** for menus/terms and **Prices** for values, and replies in text/voice/image.  
It is designed to reduce manual back‑and‑forth, standardize responses, and accelerate quoting.

---

## ⚡ Problem & Opportunity
**Problem**  
Managing WhatsApp leads is time‑consuming: repeated questions, scattered information, and manual quoting.  

**Opportunity**  
Automate the first line of attention with a **tool‑enabled agent** that retrieves official menus/conditions and current prices, keeping tone and brand consistent.

---

## 🛠️ Project Structure
```
whatsapp_catering_agent/
│── Agente-Whatsapp.json       # n8n workflow (importable)
│── docs/
│   └── Budgets.docx           # Optional local copy of the budgets brief
│── README.md                  # This document
└── .gitignore
```

---

## 🔧 Requirements
- **n8n** v1.40+
- **WhatsApp** provider (Meta WhatsApp Cloud API or Twilio WhatsApp)
- **OpenAI API Key** (chat + STT + TTS + vision, as configured in the flow)
- **Google** access for Docs & Sheets (OAuth or service account)
- **MCP endpoints** for the tools:
  - **Budgets** → exposes Google Docs “get documents”
  - **Prices** → reads pricing from Google Sheets (or via node)

> Store all secrets in **n8n Credentials**. Do **not** commit keys to the repo.

---

## ▶️ How to Use
1. **Import** the workflow: *Workflows → Import from file* → `Agente-Whatsapp.json`.
2. **Wire credentials** node‑by‑node:
   - WhatsApp Trigger / Send → WhatsApp credentials (Phone Number ID, token).
   - OpenAI nodes → OpenAI API Key.
   - HTTP Request (media download) → Bearer token if needed.
   - Google Sheets (Prices) → OAuth 2 / Service Account with access to the sheet.
   - MCP Client (Budgets) → Endpoint URL pointing to the Google Docs tool.
3. **Set constants** where needed (phone number ID, doc/sheet IDs, sheet name/range).
4. **Activate** the workflow and test with text, voice, and image messages.

---

## 🔑 Required Credentials
- **WhatsApp API** (Cloud API/Twilio)  
- **OpenAI API Key**  
- **Google OAuth / Service Account** (Docs + Sheets)  
- **MCP endpoints** for **Budgets** and **Prices**  

> Keep `.env`, tokens, and exported credentials **out of version control**.

---

## 📊 Example Input / Output
**User (text):** “¿Tienen *Finger Food* para 80 personas el 12/10 en Salta Capital?”  
**Agent:** queries **Budgets** (menus/conditions) + **Prices** (80 guests, Finger Food) → returns a concise quote and next steps.

**User (voice):** asks for menus → agent transcribes, queries tools, and **replies with voice** (TTS).  

**User (image + caption):** sends a venue photo → agent analyzes/acknowledges and shares availability info via text.

---

## 👨‍💻 Author
**Francisco Moyano Escalera**  
Specialist in Data, AI & Automation  
📧 frannmmm419@gmail.com  
🌐 GitHub: [franm419](https://github.com/franm419)

