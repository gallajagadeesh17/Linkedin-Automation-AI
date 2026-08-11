# 🚀 LinkedIn Automation AI

> **Turn a topic into a LinkedIn-ready post, review it with a human, regenerate when needed, and publish only after approval.**

[![n8n](https://img.shields.io/badge/Automation-n8n-EA4B71?logo=n8n&logoColor=white)](https://n8n.io/)
[![Google Gemini](https://img.shields.io/badge/AI-Google%20Gemini-4285F4?logo=google&logoColor=white)](https://ai.google.dev/)
[![Gmail](https://img.shields.io/badge/Approval-Gmail-EA4335?logo=gmail&logoColor=white)](https://gmail.com/)
[![LinkedIn](https://img.shields.io/badge/Publishing-LinkedIn-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/)

## 🎯 Product Overview

**LinkedIn Automation AI** is a human-in-the-loop content automation agent built in n8n. A user submits a topic through the workflow chat, Google Gemini creates a structured LinkedIn post, and Gmail presents the draft for review.

The user remains in control of publication:

- **Approve** → publish to LinkedIn
- **Regenerate** → rewrite the post using feedback
- **Reject** → discard the draft

The core workflow is implemented with an n8n chat trigger, AI agents, Google Gemini models, Gmail approval steps, JavaScript transformation nodes, and LinkedIn publishing nodes. fileciteturn14file0L1-L2

> 📋 **Product Requirements Document:** [docs/PRD.md](docs/PRD.md)

---

## 🧩 The Problem

Creating consistent LinkedIn content involves repeated work:

**Topic → Writing → Formatting → Reviewing → Editing → Publishing**

The goal of this automation is to reduce that manual effort while keeping a human approval checkpoint before anything is published.

## 💡 The Solution

```text
┌─────────────────────┐
│ 💬 User enters topic│
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│ 🤖 Gemini AI Agent  │
│ Generate post       │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│ 📧 Gmail Review     │
│ Approve / Regenerate │
│ / Reject             │
└──────┬──────┬───────┘
       │      │
   Approve  Regenerate
       │      ↓
       │  🤖 Gemini Rewrite
       │      ↓
       │  📧 Review Again
       │
       ↓
┌─────────────────────┐
│ 🔗 LinkedIn Publish │
└─────────────────────┘

Reject → 🗑️ Discard
```

## ✨ Current Capabilities

### 1. 🤖 AI Post Generation
The initial Gemini agent is instructed to create a LinkedIn post with a defined structure: hook, explanation, why it matters, key benefits, key takeaways, an engaging question, a call to action, and relevant hashtags. The workflow also constrains the draft to a maximum of 220 words and short paragraphs. fileciteturn10file0L5-L9

### 2. 📧 Human Approval
The first Gmail approval step shows the generated post and provides three decisions: **Yes**, **No**, and **Regenerate**, plus an **Edit post** field for feedback. fileciteturn10file0L79-L120

### 3. 🔄 Feedback-Based Regeneration
When regeneration is selected, a second AI agent receives the original post and the user's feedback, then creates an improved version for another approval cycle. fileciteturn10file0L238-L251

### 4. ✅ Controlled Publishing
The switch node routes an approved draft toward the LinkedIn publishing node, while the rejection path sends a discard response. fileciteturn10file0L498-L533

### 5. 🧠 AI Memory
The workflow includes an n8n Simple Memory node connected to the initial AI agent. fileciteturn10file0L618-L624

---

## 🏗️ Architecture

| Layer | Component | Responsibility |
|---|---|---|
| Input | n8n Chat Trigger | Receives the user's topic |
| Intelligence | AI Agent + Gemini | Generates the LinkedIn draft |
| Formatting | JavaScript | Validates/transforms generated content |
| Approval | Gmail | Collects human decision and feedback |
| Decision | Switch / If | Routes approve, regenerate, or reject paths |
| Revision | AI Agent + Gemini | Rewrites content from feedback |
| Publishing | LinkedIn node | Publishes the approved post |
| Memory | Simple Memory | Provides conversation memory to the initial agent |

## 🔄 Decision Flow

| Decision | Result |
|---|---|
| ✅ **Yes** | Publish the current post to LinkedIn |
| 🔄 **Regenerate** | Send original post + feedback to Gemini and request a new version |
| ❌ **No** | Discard the post |

The regeneration branch returns the revised post to a second Gmail approval checkpoint before the final LinkedIn publishing decision. fileciteturn10file0L534-L590

---

## 📝 Content Contract

The current AI prompt is intentionally structured around a consistent LinkedIn format:

```text
🚀 Hook

Topic explanation

💡 Why it matters

✅ Key Benefits

📌 Key Takeaways

❓ Engagement question

👇 Share your thoughts!

#Hashtags
```

Current generation rules include:

- Maximum **220 words**
- Short 1–2 line paragraphs
- Blank lines between sections
- Plain text output
- No HTML
- No Markdown
- No JSON
- Natural emoji usage
- An ending question
- 8–10 relevant hashtags

These are derived from the workflow's configured AI prompt rather than being generic product assumptions. fileciteturn10file0L5-L9

---

## 🛠️ Tech Stack

- **n8n** — workflow orchestration
- **Google Gemini** — AI content generation and rewriting
- **Gmail** — human approval and feedback
- **LinkedIn** — publishing
- **JavaScript** — content transformation
- **n8n AI Agent** — agentic content generation
- **Simple Memory** — conversational context

---

## 📁 Repository Structure

```text
Linkedin-Automation-AI/
│
├── workflows/
│   └── linkedin-automation-ai.json
│
├── docs/
│   └── PRD.md
│
└── README.md
```

---

## 🚀 Setup

### 1. Open n8n
Use n8n Cloud or a local n8n installation.

### 2. Import the workflow

Import:

```text
workflows/linkedin-automation-ai.json
```

### 3. Configure integrations

Connect the following inside n8n:

- Google Gemini
- Gmail
- LinkedIn

### 4. Configure workflow-specific values

The public workflow uses placeholders instead of personal credentials:

```text
YOUR_GEMINI_CREDENTIAL_ID
YOUR_GMAIL_CREDENTIAL_ID
YOUR_LINKEDIN_CREDENTIAL_ID
YOUR_LINKEDIN_PERSON_ID
YOUR_EMAIL
```

### 5. Test safely

Start with a test LinkedIn account or controlled workflow execution. Verify:

```text
Topic
  ↓
Gemini Draft
  ↓
Gmail Approval
  ↓
Approve / Regenerate / Reject
  ↓
LinkedIn
```

---

## 🔐 Security & Privacy

Do **not** commit:

- API keys
- OAuth tokens
- Gmail addresses
- LinkedIn account identifiers
- Webhook signatures
- n8n credential IDs from private environments

Use n8n's credential management for secrets. This repository intentionally stores placeholders instead of the original account-specific credential values.

---

## 📊 Product Success Metrics

The current workflow does not contain analytics or KPI tracking. The following are **proposed product metrics** for future versions:

| Metric | What it measures |
|---|---|
| Draft approval rate | Percentage of generated posts approved without rejection |
| Regeneration rate | Percentage of drafts requiring another AI pass |
| Time to publish | Time from topic submission to approved publication |
| Edit frequency | How often users provide manual feedback |
| Publish completion rate | Percentage of workflows reaching LinkedIn publishing |

These are proposed measurements, not metrics currently collected by the workflow.

---

## 🔮 Roadmap

### Phase 1 — Current
- [x] Topic input through n8n chat
- [x] Gemini content generation
- [x] Structured LinkedIn post format
- [x] Gmail approval
- [x] Regeneration with feedback
- [x] Reject/discard path
- [x] LinkedIn publishing

### Phase 2 — Planned
- [ ] Content history
- [ ] Brand voice configuration
- [ ] Scheduled publishing
- [ ] Draft storage
- [ ] Post analytics
- [ ] Content calendar

### Phase 3 — Advanced
- [ ] Multiple LinkedIn profiles
- [ ] AI-generated post images
- [ ] Performance-aware content recommendations
- [ ] Topic/content recommendations
- [ ] Approval dashboard

---

## 📋 Documentation

- **Workflow:** [`workflows/linkedin-automation-ai.json`](workflows/linkedin-automation-ai.json)
- **Product Requirements:** [`docs/PRD.md`](docs/PRD.md)

## 👨‍💻 Author

**Jagadeesh Galla**

Built with **n8n + Google Gemini + Gmail + LinkedIn**.

---

⭐ If this project is useful, consider starring the repository.
