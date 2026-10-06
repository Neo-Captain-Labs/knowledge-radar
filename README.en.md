# Knowledge Radar

[繁體中文](README.md) | [Implementation specification](SPEC.en.md) | [AI Agent task brief](AGENT_TASK.en.md)

Knowledge Radar is a personal workflow for collecting incoming reading, applying an AI first-pass against your own criteria, and delivering a short briefing where you will actually see it.

It does not decide what matters for you. It reduces the work of sorting through everything before you can decide.

Chinese introduction: [Build a personal AI knowledge radar](https://neohime.com/ai-agent-knowledge-radar/)

## Flow

```text
RSS / newsletters / topic feeds
            ↓
       Reading inbox
            ↓
   AI triage using your rules
            ↓
 Deduplication, grouping, briefing
            ↓
 Discord / LINE / Slack / email
            ↓
 You decide what to read or act on
```

## Start small

1. Run a manual version for several days.
2. Record every false positive and every missed item.
3. Turn those observations into explicit inclusion, exclusion, and grouping rules.
4. Automate only when the briefing is consistently useful.

The hard part is not deployment. It is defining the signal you want to keep.

## Documentation

| File | Purpose |
| --- | --- |
| [SPEC.en.md](SPEC.en.md) | English implementation specification. |
| [SPEC.md](SPEC.md) | Traditional Chinese implementation specification. |
| [AGENT_TASK.en.md](AGENT_TASK.en.md) | Copy-ready brief for an AI coding agent. |
| [SECURITY.md](SECURITY.md) | Credential, privacy, and publishing rules. |

## License

Documentation is licensed under [CC BY 4.0](LICENSE).
