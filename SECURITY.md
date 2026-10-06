# Security

This repository is designed to be public. Treat every tracked file and every Git commit as public material.

## Never commit

- API keys, access tokens, application passwords, OAuth refresh tokens, cookies, or private keys
- Discord, Slack, LINE, Telegram, or other notification webhook URLs
- Personal reading-inbox exports, source lists, highlights, notes, or filtering profiles
- Cloud project identifiers that reveal private infrastructure
- Screenshots containing account identifiers or secrets
- Real `.env` files

## Required practices

1. Keep `.env` ignored by Git and commit only `.env.example` with empty values.
2. Use a managed secret store in cloud deployments.
3. Give service accounts the smallest permissions needed.
4. Require authenticated requests for scheduled job endpoints.
5. Inspect staged changes before every commit.
6. Rotate a credential immediately if it appears in a commit, log, screenshot, or public issue.

## Public examples

Use generic placeholders such as `[YOUR_READING_API_TOKEN]` or `https://example.com/webhook/...`. Do not redact a real secret after it has already been committed; revoke it first.
