# Product Requirements Document (PRD)

## LinkedIn Automation AI

**Version:** 1.0  
**Status:** MVP / Working Prototype  
**Platform:** n8n  
**Primary Integrations:** Google Gemini, Gmail, LinkedIn

---

## 1. Product Summary

LinkedIn Automation AI is an AI-assisted LinkedIn content automation agent designed to reduce the repetitive work involved in creating and publishing professional LinkedIn posts.

The product accepts a topic, generates a structured LinkedIn post with Google Gemini, sends the draft to the user for human review through Gmail, and only publishes after approval. If the user is not satisfied, the workflow can regenerate the content using user feedback.

The current implementation is a workflow automation prototype rather than a standalone SaaS application.

## 2. Problem Statement

Professionals often spend time deciding what to post, writing the content, formatting it for LinkedIn, reviewing the draft, making edits, and finally publishing it.

The product addresses this workflow by automating content creation while preserving a human approval step before publication.

> **How might we automate repetitive LinkedIn content creation and publishing without removing the user's control over what gets published?**

## 3. Product Vision

Create a reliable AI content assistant that can take a simple topic and transform it into high-quality, reviewable LinkedIn content with minimal manual effort.

The long-term vision is to evolve from a single workflow into a personalized LinkedIn content copilot with brand voice, scheduling, analytics, content history, and performance-aware recommendations.

## 4. Goals

### Primary Goals

1. Generate a LinkedIn-ready post from a simple topic.
2. Maintain a consistent content structure and professional tone.
3. Give the user a clear approval checkpoint before publishing.
4. Allow the user to regenerate content using feedback.
5. Publish approved content to LinkedIn automatically.
6. Keep credentials and personal account information out of the public repository.

### Secondary Goals

1. Reduce time spent manually drafting posts.
2. Reduce repetitive editing and formatting work.
3. Create a reusable automation architecture that can be extended later.

## 5. Non-Goals for MVP

The current MVP does **not** aim to provide:

- LinkedIn analytics
- Automated content scheduling
- Multiple account management
- A web dashboard
- Content calendar management
- AI image generation
- Automatic performance optimization
- Persistent content history
- Full social-media management across multiple platforms

## 6. Target Users

### Primary Persona — Professional / Job Seeker

**Needs:**
- Maintain a professional LinkedIn presence
- Share learning and career-related topics
- Reduce time spent writing posts
- Keep control over published content

**Pain Points:**
- Writer's block
- Repetitive formatting
- Lack of time
- Multiple rounds of editing

### Secondary Persona — Creator / Builder

**Needs:**
- Generate technical or educational content quickly
- Experiment with AI-assisted content workflows
- Maintain consistent posting

## 7. User Journey

```text
1. User opens the n8n chat interface
             ↓
2. User enters a topic
             ↓
3. AI Agent sends topic to Gemini
             ↓
4. Gemini generates LinkedIn draft
             ↓
5. Draft is sent through Gmail
             ↓
6. User chooses:
      ├── Approve
      ├── Regenerate + feedback
      └── Reject
             ↓
7A. Approve → LinkedIn publication
7B. Regenerate → Gemini rewrite → Gmail review
7C. Reject → Discard response
```

## 8. Functional Requirements

### FR-01 — Topic Input

The system must accept a user topic through the n8n chat trigger.

**Example:** `Benefits of AI automation for small businesses`

### FR-02 — AI Draft Generation

The system must use Google Gemini to generate a LinkedIn post from the supplied topic.

The configured prompt requires:

- A short hook
- Topic explanation
- Why the topic matters
- Three benefits
- Three takeaways
- An engaging question
- A call to action
- 8–10 relevant hashtags

The current workflow also specifies a maximum of 220 words and short paragraphs.

### FR-03 — Draft Formatting

The generated content must be converted into clean, LinkedIn-ready text. JavaScript nodes handle inspection and text transformation, including newline handling during regeneration.

### FR-04 — Human Approval

The system must send the generated post to the user's email and wait for a decision.

The approval form must support:

- **Yes**
- **No**
- **Regenerate**
- Optional edit/feedback text

### FR-05 — Regeneration

When the user chooses **Regenerate**, the system must provide the original post and user feedback to the second AI agent. The AI agent must return a revised LinkedIn post.

### FR-06 — Second Approval

The regenerated post must be sent back to the user for another approval decision.

### FR-07 — LinkedIn Publishing

When the user approves the post, the system must send the approved text to the configured LinkedIn account through the LinkedIn n8n node.

### FR-08 — Rejection

When the user rejects a draft, the system must stop the publishing path and send a discard response.

### FR-09 — Credential Safety

Credentials must be stored through n8n's credential management rather than hard-coded into the public workflow.

Public repository files must not contain real API keys, OAuth tokens, email addresses, LinkedIn identifiers, or webhook signatures.

## 9. AI Requirements

### AI Model

**Google Gemini**

### Initial Agent

Responsible for generating the first LinkedIn draft from the user's topic.

### Regeneration Agent

Responsible for improving or regenerating the original draft based on user feedback.

### Content Rules

The current prompt requires:

- Professional tone
- Short paragraphs
- Natural emojis
- Plain text
- No HTML
- No Markdown
- No JSON
- Ending with a question
- 8–10 relevant hashtags
- Maximum 220 words

## 10. Human-in-the-Loop Design

Human approval is a core product requirement.

AI-generated content can be technically correct but still fail to match a user's voice, professional context, intent, preferred tone, or current situation. The approval step gives the user final control before the content reaches LinkedIn.

```text
DRAFT
  │
  ├── APPROVED ────────→ PUBLISHED
  │
  ├── REGENERATE ──────→ REVISED DRAFT
  │                           │
  │                           └── APPROVAL
  │
  └── REJECTED ────────→ DISCARDED
```

## 11. Non-Functional Requirements

### Reliability

The workflow should fail safely rather than publish content when the approval state is unclear.

### Security

Credentials must remain outside public source files.

### Maintainability

Nodes should have descriptive names and the workflow should remain modular enough to replace individual integrations.

### Usability

The core interaction should remain simple:

**Topic → Review → Decision**

### Observability

Future versions should record execution outcomes and publishing failures without storing unnecessary personal data.

## 12. Success Metrics

The MVP does not currently collect analytics. The following are proposed product KPIs:

| KPI | Definition | Target Direction |
|---|---|---|
| Draft approval rate | Approved drafts / generated drafts | ↑ |
| Regeneration rate | Regenerated drafts / generated drafts | ↓ |
| Publish completion rate | Published drafts / started workflows | ↑ |
| Time to publish | Topic submission → successful publication | ↓ |
| Manual edit rate | Drafts requiring user edits | ↓ |
| Workflow failure rate | Failed executions / total executions | ↓ |

## 13. Edge Cases

### No AI output
The workflow should not publish an empty post.

### User rejects draft
Publishing must stop.

### Repeated regeneration
The workflow should eventually provide a controlled termination path rather than creating an uncontrolled loop.

### Gmail response unavailable
The workflow should remain pending or fail safely without publishing.

### LinkedIn publishing fails
The workflow should surface the failure and avoid reporting the post as successfully published.

### Invalid credentials
The workflow should fail with a clear configuration error.

## 14. Future Product Enhancements

### Content Management

- Save generated drafts
- Search previous posts
- Duplicate successful posts
- Content categories

### Personalization

- User-defined brand voice
- Tone selector
- Audience selector
- Industry profile
- Writing style memory

### Scheduling

- Content calendar
- Scheduled publishing
- Best-time recommendations

### Analytics

- Impressions
- Likes
- Comments
- Reposts
- Engagement rate
- Post-level performance history

### Intelligence

- Topic recommendations
- Trending-topic discovery
- Performance-aware generation
- A/B post variations

### Experience

- Web dashboard
- Approval inbox
- Mobile-friendly approval
- Multi-account support

## 15. MVP Acceptance Criteria

- [ ] A user can submit a topic.
- [ ] Gemini generates a LinkedIn draft.
- [ ] The draft follows the configured content structure.
- [ ] Gmail sends the draft for approval.
- [ ] The user can approve the draft.
- [ ] An approved draft can reach LinkedIn publishing.
- [ ] The user can reject the draft.
- [ ] Rejected drafts are not published.
- [ ] The user can request regeneration.
- [ ] Feedback is passed to the regeneration agent.
- [ ] The regenerated draft returns for approval.
- [ ] Real credentials are not stored in the public repository.

## 16. Current Workflow Mapping

| Product Requirement | Current Implementation |
|---|---|
| Topic input | n8n Chat Trigger |
| AI generation | AI Agent + Google Gemini |
| Memory | Simple Memory |
| Formatting | JavaScript nodes |
| Approval | Gmail Send & Wait |
| Decision routing | Switch / If |
| Regeneration | Second AI Agent + Gemini |
| Final approval | Gmail Send & Wait |
| Publishing | LinkedIn node |
| Rejection | Gmail response |

## 17. Product Principle

> **Automate the work, not the user's judgment.**

The agent should handle repetitive content creation and formatting while the user retains control over the final message and publication.

## 18. Future Vision

The long-term product can evolve from a simple LinkedIn automation workflow into an **AI-powered personal content copilot** that understands the user's professional voice, recommends topics, creates content, learns from feedback, schedules posts, and measures results — while keeping publication approval under the user's control.
