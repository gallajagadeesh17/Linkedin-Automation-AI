# Security Policy

## Scope

This repository contains an n8n workflow for AI-assisted LinkedIn content creation, human approval, and publishing.

## Secrets

Never commit:

- Google Gemini API keys
- Gmail OAuth tokens
- LinkedIn OAuth tokens
- n8n credential secrets
- Personal email addresses
- Private webhook URLs or signatures
- Private repository data

Use n8n's credential manager for authentication details.

## Public Workflow Safety

The workflow published in this repository uses placeholders for account-specific values. Configure your own credentials after importing it into n8n.

## Reporting a Security Issue

If you discover a credential, token, or other sensitive information in this repository, do not reproduce it in a public issue. Remove access or rotate the credential immediately and contact the repository owner through GitHub.

## Best Practices

1. Use least-privilege credentials where possible.
2. Use read/write permissions only where the workflow requires them.
3. Rotate exposed credentials immediately.
4. Test automation on a controlled account before enabling production publishing.
5. Review AI-generated content before publication.
