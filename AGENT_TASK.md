# Task Brief for an AI Coding Agent

Copy the brief below into an AI coding agent after you have completed the configuration answers in [SPEC.md](SPEC.md).

```text
Build a personal knowledge-radar service according to SPEC.md in this repository.

Before writing code:
1. Read SPEC.md and SECURITY.md completely.
2. Ask me for any missing decisions listed in SPEC.md section 2.
3. Present an implementation plan, the files you will create, the services that require my own accounts, and every permission or credential I must provide myself.
4. Do not publish anything, create cloud resources, send notifications, or spend money until I explicitly approve that exact action.

Implementation constraints:
- Keep provider integrations modular. The reading source, LLM, scheduler, and notification channel must be replaceable.
- Read configuration only from environment variables or a protected secret manager.
- Provide .env.example with names only. Never print, commit, or copy real credentials.
- Keep the first-pass input to metadata and summaries; do not fetch full articles by default.
- Record last_successful_briefing_at only after successful delivery.
- Group duplicate coverage and enforce the configured maximum item count.
- Output a source link and, when available, a reading-inbox link for each retained item.
- Write tests for duplicate grouping, state advancement after successful delivery, and message-length splitting.
- Document local development, deployment assumptions, rollback steps, and ongoing maintenance.

At each milestone, explain what changed and what I need to verify before moving on.
```
