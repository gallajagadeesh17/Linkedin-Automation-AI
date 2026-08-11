# Setup Guide

## Prerequisites

You need:

- An n8n Cloud or self-hosted n8n instance
- A Google Gemini credential
- A Gmail OAuth credential
- A LinkedIn OAuth credential
- Permission to publish from the configured LinkedIn account

## 1. Import the Workflow

Import:

```text
workflow/linkedin-automation-ai.json
```

into n8n.

## 2. Configure Google Gemini

Create or connect the Google Gemini credential used by the two AI agents.

The initial agent creates the first draft. The revision agent improves or regenerates a draft using user feedback.

## 3. Configure Gmail

Connect Gmail OAuth to both approval nodes.

Configure the destination email used for approval. The Gmail nodes use n8n's **Send and Wait for Response** operation so the workflow pauses until the user submits a decision.

## 4. Configure LinkedIn

Connect the LinkedIn OAuth credential to both LinkedIn publishing nodes.

The first node publishes the initially approved draft. The second publishes the revised draft after its final approval.

## 5. Replace Placeholders

The public workflow intentionally uses placeholders such as:

```text
YOUR_GEMINI_CREDENTIAL_ID
YOUR_GMAIL_CREDENTIAL_ID
YOUR_LINKEDIN_CREDENTIAL_ID
YOUR_LINKEDIN_PERSON_ID
YOUR_EMAIL
```

Use your actual n8n credential configuration. Do not put API keys or OAuth tokens directly into the JSON file.

## 6. Run a Controlled Test

Use a non-critical test topic first:

```text
How AI automation can help students learn faster
```

Verify the complete path:

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

## 7. Test Regeneration

Choose **Regenerate** and enter specific feedback such as:

```text
Make the opening stronger and make the post more technical.
```

Confirm that the revision agent receives the original post and feedback, then sends the revised post to the second approval step.

## 8. Test Rejection

Choose **No** and verify that the workflow stops the publishing path and sends the discard response.

## Troubleshooting

### Gemini does not generate a post

Check the Google Gemini credential and confirm the model node is connected to the appropriate AI Agent.

### Gmail approval does not arrive

Check Gmail OAuth permissions, recipient configuration, and the Send and Wait node settings.

### LinkedIn does not publish

Check LinkedIn OAuth, the configured person identifier, and the LinkedIn node configuration.

### Regenerated content contains formatting issues

Check the JavaScript formatting node between the revision agent and the second approval node.

## Security Checklist

- [ ] No API keys in workflow JSON
- [ ] No OAuth tokens in GitHub
- [ ] No personal email address committed
- [ ] LinkedIn identifiers replaced with placeholders
- [ ] n8n credentials configured through the credential manager
- [ ] Test approval before enabling real publishing
