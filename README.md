<div align="center">

# 🚀 LinkedIn Automation AI

### Human-in-the-loop AI content creation and publishing with n8n

[![n8n](https://img.shields.io/badge/n8n-AI_Automation-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io/)
[![Google Gemini](https://img.shields.io/badge/Google_Gemini-AI-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev/)
[![Gmail](https://img.shields.io/badge/Gmail-Human_Approval-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](https://gmail.com/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Publishing-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](LICENSE)

**Give the agent a topic. Gemini writes the post. You review it. Only approved content reaches LinkedIn.**

[Quick Start](docs/SETUP.md) · [Architecture](docs/ARCHITECTURE.md) · [PRD](docs/PRD.md) · [Workflow](workflow/linkedin-automation-ai.json)

</div>

---

## 🎯 What is LinkedIn Automation AI?

**LinkedIn Automation AI** is an n8n-based AI content automation agent that turns a simple topic into a structured LinkedIn post, sends it to the user for review through Gmail, supports feedback-based regeneration, and publishes only after approval.

The key product principle is simple:

> **Automate the work, not the user's judgment.**

The current implementation is a working automation prototype, not a standalone SaaS application.

## 🧩 The Problem

Creating professional LinkedIn content repeatedly involves:

```text
Topic → Research/Idea → Writing → Formatting → Reviewing → Editing → Publishing
```

This creates repetitive work and makes consistent posting harder.

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
       │      └──────────────┐
       │                     ↓
       │              🤖 Revision Agent
       │                     ↓
       │              📧 Review Again
       │                     │
       ↓                     ↓
┌─────────────────────────────────────┐
│ 🔗 LinkedIn — Publish only if      │
│    the user approves the content   │
└─────────────────────────────────────┘

Reject → 🗑️ Discard
```

## ✨ Core Capabilities

| Capability | What it does |
|---|---|
| 🤖 AI Generation | Creates a structured LinkedIn post from a topic |
| 📧 Human Approval | Sends the draft to Gmail and waits for a decision |
| 🔄 Regeneration | Rewrites the post using the user's feedback |
| ❌ Rejection | Stops the publishing path |
| 🔗 LinkedIn Publishing | Publishes an approved post |
| 🧠 AI Memory | Provides conversational memory to the initial agent |
| ⚡ n8n Orchestration | Connects the complete workflow |

## 🧠 Agent Design

### Content Generation Agent

Creates the initial post from the user's topic using Google Gemini.

### Revision Agent

Receives the original post and user feedback when regeneration is requested. It produces an improved version for another approval cycle.

### Human Approval Layer

The user is the final decision-maker:

```text
AI Draft
   ↓
Human Review
   ├── ✅ Approve → Publish
   ├── 🔄 Regenerate → AI Revision → Review Again
   └── ❌ Reject → Stop
```

This prevents the AI from having unconditional publishing authority.

## 📝 Content Contract

The current generation prompt asks the AI to structure posts around:

- 🚀 Hook
- Topic explanation
- 💡 Why it matters
- ✅ Key benefits
- 📌 Key takeaways
- ❓ Engagement question
- 👇 Call to action
- #️⃣ 8–10 relevant hashtags

Current generation constraints include a **maximum of 220 words**, short paragraphs, plain text output, natural emoji usage, and no HTML, Markdown, or JSON.

These are the workflow's configured generation rules, not LinkedIn platform requirements.

## 🏗️ Architecture

| Layer | Component | Responsibility |
|---|---|---|
| Input | n8n Chat Trigger | Receives the content topic |
| Intelligence | AI Agent + Gemini | Generates the initial draft |
| Transformation | JavaScript | Inspects and formats generated content |
| Approval | Gmail | Collects human approval and feedback |
| Decision | Switch / If | Routes approve, regenerate, or reject |
| Revision | AI Agent + Gemini | Improves content using feedback |
| Publishing | LinkedIn | Publishes approved content |
| Memory | Simple Memory | Provides initial agent context |

See the detailed [Architecture & Node Rationale](docs/ARCHITECTURE.md).

## 🔄 Decision Matrix

| User Decision | Workflow Result |
|---|---|
| ✅ Yes | Publish current draft to LinkedIn |
| 🔄 Regenerate | Send original + feedback to revision agent |
| ❌ No | Discard and stop publishing |

## 🚀 Quick Start

### 1. Open n8n

Use n8n Cloud or a self-hosted n8n instance.

### 2. Import the workflow

Import:

```text
workflow/linkedin-automation-ai.json
```

### 3. Configure integrations

Connect:

- Google Gemini
- Gmail
- LinkedIn

### 4. Configure placeholders

The public workflow intentionally uses placeholders such as:

```text
YOUR_GEMINI_CREDENTIAL_ID
YOUR_GMAIL_CREDENTIAL_ID
YOUR_LINKEDIN_CREDENTIAL_ID
YOUR_LINKEDIN_PERSON_ID
YOUR_EMAIL
```

Configure credentials through n8n rather than putting secrets into the workflow JSON.

### 5. Test the complete flow

```text
Topic
  ↓
Gemini Draft
  ↓
Gmail Approval
  ↓
Approve / Regenerate / Reject
  ↓
LinkedIn or Stop
```

For detailed instructions and troubleshooting, see the [Setup Guide](docs/SETUP.md).

## 📁 Repository Structure

```text
Linkedin-Automation-AI/
│
├── docs/
│   ├── ARCHITECTURE.md
│   ├── PRD.md
│   └── SETUP.md
│
├── workflow/
│   └── linkedin-automation-ai.json
│
├── .gitignore
├── LICENSE
├── README.md
└── SECURITY.md
```

## 📋 Documentation

| Document | Purpose |
|---|---|
| [PRD.md](docs/PRD.md) | Product requirements, personas, goals, KPIs, acceptance criteria, and roadmap |
| [ARCHITECTURE.md](docs/ARCHITECTURE.md) | Workflow architecture and node-by-node rationale |
| [SETUP.md](docs/SETUP.md) | Import, credential configuration, testing, and troubleshooting |
| [SECURITY.md](SECURITY.md) | Secrets, credential, and safe deployment guidance |
| [Workflow JSON](workflow/linkedin-automation-ai.json) | Importable n8n workflow |

## 🔐 Security

Never commit:

- API keys
- OAuth tokens
- Gmail credentials
- LinkedIn account identifiers
- Private webhook secrets
- n8n credential secrets

Use n8n's credential manager and follow the [Security Policy](SECURITY.md).

## 📊 Product Metrics

The current workflow does **not** collect analytics. These are proposed future KPIs:

| KPI | Definition |
|---|---|
| Draft approval rate | Approved drafts ÷ generated drafts |
| Regeneration rate | Regenerated drafts ÷ generated drafts |
| Publish completion rate | Published workflows ÷ started workflows |
| Time to publish | Topic submission → successful publication |
| Workflow failure rate | Failed executions ÷ total executions |

## 🗺️ Roadmap

### Phase 1 — Current MVP

- [x] Topic input
- [x] Gemini content generation
- [x] Structured LinkedIn format
- [x] Gmail approval
- [x] Feedback-based regeneration
- [x] Reject/discard path
- [x] LinkedIn publishing

### Phase 2 — Planned

- [ ] Content history
- [ ] Brand voice profiles
- [ ] Scheduled publishing
- [ ] Content calendar
- [ ] Draft storage
- [ ] Analytics

### Phase 3 — Advanced

- [ ] Multiple LinkedIn profiles
- [ ] AI-generated post images
- [ ] Topic recommendations
- [ ] Performance-aware generation
- [ ] Approval dashboard
- [ ] Engagement insights

## 🧪 Known MVP Boundaries

The current workflow does not provide a standalone web dashboard, LinkedIn analytics, scheduling, persistent content history, or multi-account management. These are roadmap items rather than current features.

AI output should always be reviewed before publication. LinkedIn publication also depends on valid n8n and LinkedIn credentials.

## 📜 License

This project is released under the [MIT License](LICENSE).

## 👨‍💻 Author

**Galla jagadeesh**

Built with **n8n + Google Gemini + Gmail + LinkedIn**.

---

<div align="center">

⭐ If you find this project useful, consider starring the repository.

</div>
