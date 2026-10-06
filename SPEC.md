# Knowledge Radar v4 Specification

## 1. Objective

Build a personal system that collects incoming information, performs a configurable relevance triage, and sends a compact briefing. The system must help a person decide what to read next. It must not pretend to make the final decision for them.

The specification is provider-neutral. A practical implementation may use an RSS reader with an API, an LLM provider, and one notification channel.

## 2. Decisions the user must make

Before implementation, collect these answers. Do not infer them from a sample configuration.

| Decision | Questions to ask |
| --- | --- |
| Topics | What areas should be retained? What should be excluded? |
| Sources | Which sites, newsletters, channels, or topic feeds should enter the reading inbox? |
| Delivery | Which channel is checked every day: Discord, LINE, Slack, Telegram, or email? |
| Cadence | What time and timezone should the briefing run? |
| Output size | How many items are useful on a normal day? |
| Language | What language should the briefing use? |
| Automation level | Manual, local scheduled, or cloud scheduled? |
| Feedback | Which actions should signal keep, ignore, research, or turn into content? |

## 3. Architecture

```text
Sources
  ├─ RSS and Atom feeds
  ├─ newsletters
  ├─ video-channel feeds
  └─ topic-search feeds
        ↓
Reading inbox
  ├─ retains original material
  ├─ supports reading across devices
  └─ keeps highlights, notes, and tags
        ↓
Triage job
  ├─ reads unseen items in the time window
  ├─ removes obvious noise
  ├─ groups duplicate coverage
  ├─ ranks remaining items
  └─ writes a briefing in the requested language
        ↓
Notification channel
  ├─ compact message only
  ├─ original-source link
  └─ reading-inbox link when available
        ↓
Human review and feedback
```

## 4. Data model

Normalise each incoming item before triage.

```json
{
  "id": "stable-item-id",
  "title": "Original title",
  "source_name": "Source name",
  "source_url": "https://example.com/article",
  "reader_url": "https://reader.example/item",
  "published_at": "2026-01-01T00:00:00Z",
  "summary": "Feed summary, if available",
  "unread": true,
  "tags": [],
  "notes": []
}
```

Do not require the LLM to read full articles during the first pass. Use title, source, date, and feed summary unless the user deliberately selects an item for deeper work.

## 5. Triage rules

The implementation must receive explicit user-defined inclusion and exclusion rules. A default prompt may be structured as follows:

```text
You are an editorial triage assistant.

Keep only items that match one or more configured focus areas.
Exclude items that match configured noise patterns or have no actionable relevance.
Group reporting about the same event and retain the strongest source.
Rank items by direct relevance, novelty, urgency, and evidence quality.
Return no more than [MAX_ITEMS] items.

For each retained item, provide:
1. a title in [OUTPUT_LANGUAGE]
2. source name
3. one-sentence summary
4. one-sentence explanation of why it matters to this user
5. source and reading-inbox links

Do not invent facts. If the supplied metadata is insufficient, say so.
```

### 5.1 Duplicate handling

- If several sources cover the same event, keep one primary item.
- Prefer the most direct source, the clearest explanation, or the source with the most useful details.
- Optionally list corroborating sources inside the retained item. Do not create separate brief entries for each duplicate.

### 5.2 Priority handling

Use a small priority scale. Example:

| Priority | Meaning |
| --- | --- |
| High | Directly changes an active project, a decision, or a time-sensitive opportunity. |
| Medium | Relevant professional signal or useful method, without an immediate action. |
| Low | Worth knowing, but no action is needed now. |

Do not mark everything as high priority. A briefing with too many urgent items is not a triage system.

## 6. Briefing format

Use fixed category order so a reader can scan it quickly. Adapt categories to the user's work.

```text
Knowledge Radar | YYYY-MM-DD

Today’s signals
- [Translated title] | Source
  Why it matters: …
  Source: …
  Read later: …

Tools and methods
- …

Design and industry
- …
```

Requirements:

- Keep the full briefing within the destination platform's message limits.
- Split messages cleanly when the platform has a length limit.
- Preserve an original-source link.
- Include the reading-inbox link when it supports later reading, highlights, or notes.
- Do not send a message merely to say there are no results unless the user asks for that behavior.

## 7. Scheduling and state

### Manual mode

The agent runs only when asked. Store the timestamp of the last successful briefing. On the next run, collect items between that timestamp and now, with a maximum lookback window.

### Scheduled mode

Run at the user's chosen time and timezone. A cloud scheduler should authenticate to the job endpoint. The endpoint must not be openly callable without authentication.

### State rule

Update `last_successful_briefing_at` only after the notification has been delivered successfully. If delivery fails, retain the old value so the next run does not silently skip material.

## 8. Feedback loop

The feedback loop should improve the criteria, not remove human control.

| Human action | Suggested meaning |
| --- | --- |
| Save or highlight | Useful reference or possible knowledge note. |
| Mark as content idea | Send to a writing or production backlog. |
| Ignore | Evidence that the current rule is too broad. |
| Add a note | A richer preference signal than a click alone. |

Rules:

- Read both document-level notes and highlight-level notes if the reading service exposes both.
- Attribute feedback to topics or patterns before blaming an entire source.
- If a source has low selection for a period, flag it for human review. Do not automatically delete it.

## 9. Security requirements

- Store API tokens, notification webhooks, cookies, and credentials in a secret manager or local protected environment file.
- Never commit real credentials, private source lists, reader-export data, or personal filtering profiles.
- Commit `.env.example` with empty values only.
- Use a least-privilege service account where the platform supports it.
- Protect scheduled endpoints with platform-native authentication.
- Keep public documentation generic. Readers must supply their own providers and credentials.

See [SECURITY.md](SECURITY.md) for the full boundary.

## 10. Acceptance criteria

An implementation is ready for a small pilot when all conditions below are met.

- [ ] A sample set of at least 20 mixed items can be collected without exposing credentials.
- [ ] The triage output keeps no more than the configured maximum number of items.
- [ ] Duplicate reports are grouped into one item.
- [ ] Every retained item has a source, short explanation, and a reason it matters.
- [ ] The destination receives a readable message without truncation.
- [ ] A failed delivery does not advance the state timestamp.
- [ ] Secrets are absent from Git history, tracked files, logs, and screenshots.
- [ ] A person can change topics, exclusions, schedule, and destination without editing application code where practical.

## 11. Implementation sequence

1. Define topics and exclusions.
2. Run manual triage for several days and collect examples of false positives and missed signals.
3. Stabilise the output format and maximum item count.
4. Add a local schedule only when the manual workflow is useful.
5. Move the stable job to a cloud schedule only when independent delivery is needed.
6. Add feedback automation last, after the initial briefing quality is trusted.
