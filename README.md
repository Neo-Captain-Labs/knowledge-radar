# Knowledge Radar

Build a personal knowledge radar that collects information from RSS and newsletters, lets AI triage it against your own priorities, and sends a concise briefing to the channel you actually check.

This repository is a reusable specification, not a hosted product and not a copy of anyone's private workflow. You define the sources, priorities, notification channel, model provider, and level of automation.

Chinese guide: [拒絕數位成癮：打造第二大腦知識雷達](https://neohime.com/ai-agent-knowledge-radar/)

## What this system does

```text
RSS / newsletters / topic feeds
            ↓
       Reading inbox
            ↓
    AI relevance triage
            ↓
      Short daily briefing
            ↓
 Discord / LINE / Slack / email
            ↓
  You decide what to read or act on
```

The objective is not to summarize everything. It is to reduce a large incoming stream into a short list worth your attention.

## Choose an automation level

| Level | What happens | Best for |
| --- | --- | --- |
| Manual | You ask an AI assistant to triage unread items. | Testing your rules with no deployment. |
| Scheduled | A local computer runs the triage on a schedule. | A proven workflow that still depends on your computer. |
| Cloud | A scheduled cloud job runs independently. | A stable workflow you use regularly. |

Start manually. Automate only after you have enough examples of what the radar should keep and discard.

## Repository guide

| File | Purpose |
| --- | --- |
| [SPEC.md](SPEC.md) | System specification, setup sequence, security boundaries, and acceptance criteria. |
| [AGENT_TASK.md](AGENT_TASK.md) | A task brief you can paste into an AI coding agent. |
| [.env.example](.env.example) | Safe environment-variable names only. |
| [SECURITY.md](SECURITY.md) | Credential, publishing, and privacy rules. |

## Principles

1. A reading inbox preserves material. An AI agent triages it. These are different jobs.
2. Let the machine apply an explicit first-pass rule. Keep final judgment and action with a person.
3. Group duplicate coverage of one event. Do not make the reader scan the same news repeatedly.
4. Keep notification messages short and link back to the original reading environment.
5. Treat feedback as a signal about topics and criteria, not an automatic reason to delete a source.
6. Store credentials outside the repository. Commit an example file, never the real file.

## License

The documentation is released under [CC BY 4.0](LICENSE). You may adapt it with attribution.
