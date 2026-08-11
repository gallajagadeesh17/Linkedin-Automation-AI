# 🚀 LinkedIn Automation AI

> **AI-powered LinkedIn content creation, human approval, regeneration, and publishing — automated with n8n.**

[![n8n](https://img.shields.io/badge/Workflow-n8n-EA4B71?logo=n8n&logoColor=white)](https://n8n.io/)
[![Gemini](https://img.shields.io/badge/AI-Google%20Gemini-4285F4?logo=google&logoColor=white)](https://ai.google.dev/)
[![LinkedIn](https://img.shields.io/badge/Platform-LinkedIn-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/)
[![Gmail](https://img.shields.io/badge/Approval-Gmail-EA4335?logo=gmail&logoColor=white)](https://gmail.com/)

## ✨ What is this?

**LinkedIn Automation AI** is an n8n workflow that turns a simple topic into a polished LinkedIn post using Google Gemini, sends the draft to the user for approval through Gmail, and publishes the approved content directly to LinkedIn.

The workflow uses a **human-in-the-loop** approach: AI creates the content, while the user decides whether it should be published, regenerated, or rejected.

## 🧠 How it works

```text
💬 Enter a topic
       │
       ▼
🤖 Google Gemini
       │
       ▼
📝 Generate LinkedIn Post
       │
       ▼
📧 Send Draft for Approval
       │
       ├───────────────┬────────────────┐
       ▼               ▼                ▼
   ✅ Approve      🔄 Regenerate     ❌ Reject
       │               │                │
       │               ▼                ▼
       │          🤖 Gemini          🗑️ Discard
       │               │
       │               ▼
       │          📧 Review Again
       │               │
       │               ▼
       └──────────► 🔗 LinkedIn
                       │
                       ▼
                 🚀 Publish Post
```

## 🌟 Key Features

- 🤖 **AI content generation** using Google Gemini
- ✍️ **Structured LinkedIn posts** with hooks, benefits, takeaways, questions, and hashtags
- 📧 **Email approval workflow** using Gmail
- 🔄 **Regenerate or edit** posts using user feedback
- ✅ **Human approval before publishing**
- ❌ **Reject/discard option** for unwanted drafts
- 🔗 **Direct LinkedIn publishing** after approval
- 🧠 **Conversation memory** for the initial AI agent
- ⚡ **Fully automated orchestration** with n8n

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| **n8n** | Workflow automation and orchestration |
| **Google Gemini** | LinkedIn content generation and rewriting |
| **Gmail** | Human approval and feedback interface |
| **LinkedIn** | Final post publishing |
| **JavaScript** | Post formatting and data transformation |
| **n8n AI Agent** | AI-driven content generation |

## 📁 Repository Structure

```text
Linkedin-Automation-AI/
│
├── workflows/
│   └── linkedin-automation-ai.json
│
└── README.md
```

## ⚙️ Workflow Components

### 1. 💬 Topic Input
The workflow starts with the n8n chat trigger. A user provides the topic they want to turn into a LinkedIn post.

### 2. 🤖 AI Content Generation
Google Gemini generates a professional LinkedIn post following a predefined structure:

- 🚀 Hook
- 💡 Why it matters
- ✅ Key benefits
- 📌 Key takeaways
- ❓ Engagement question
- 👇 Call to action
- #️⃣ Relevant hashtags

### 3. 📧 Human Approval
The generated post is sent through Gmail using an approval form. The user can choose:

- **Yes** → Publish the post
- **Regenerate** → Improve/rewrite the post
- **No** → Discard the post

### 4. 🔄 Regeneration
When regeneration is selected, Gemini receives the original post and the user's feedback. It creates an improved version and sends it for approval again.

### 5. 🚀 LinkedIn Publishing
Once the user approves the final version, n8n sends it to LinkedIn automatically.

## 🔐 Security

**Never commit real API keys, OAuth credentials, email addresses, webhook signatures, or personal identifiers to GitHub.**

This repository uses placeholders such as:

```text
YOUR_GEMINI_CREDENTIAL_ID
YOUR_GMAIL_CREDENTIAL_ID
YOUR_LINKEDIN_CREDENTIAL_ID
YOUR_LINKEDIN_PERSON_ID
YOUR_EMAIL
```

Configure credentials securely inside n8n.

## 🚀 Setup

### 1. Open n8n
Use an n8n Cloud instance or a local n8n installation.

### 2. Import the workflow
Import:

```text
workflows/linkedin-automation-ai.json
```

### 3. Configure credentials
Connect Google Gemini, Gmail, and LinkedIn inside n8n.

### 4. Configure placeholders
Replace the placeholders with your own n8n credential references and account details.

### 5. Test
Send a topic through the chat trigger and verify:

```text
Topic → Gemini → Gmail Approval → Decision → LinkedIn
```

## 🎯 Example Use Case

**Input:**

> Why AI automation is becoming important for small businesses

Gemini creates a ready-to-publish LinkedIn post, sends it to Gmail for review, and waits for the user's decision before publishing.

## 🔄 Decision Logic

| User Choice | Workflow Action |
|---|---|
| ✅ Yes | Publish to LinkedIn |
| 🔄 Regenerate | Send feedback to Gemini and create a new version |
| ❌ No | Discard the post |

## 📌 Why Human-in-the-Loop?

The approval checkpoint prevents unwanted AI-generated content from being published automatically. The user keeps final control while n8n automates the repetitive work.

## 🔮 Future Improvements

- 📅 Schedule posts automatically
- 📊 Track LinkedIn post performance
- 🎯 Generate content from a content calendar
- 🗂️ Store approved posts in Google Sheets or a database
- 🎨 Add AI-generated images
- 🧩 Support multiple LinkedIn accounts
- 🗣️ Add custom brand voice profiles
- 📈 Generate analytics and engagement reports

## 👨‍💻 Author

**Jagadeesh Galla**

Built as an AI automation project using **n8n + Google Gemini + Gmail + LinkedIn**.

---

⭐ If you find this project useful, consider starring the repository!
