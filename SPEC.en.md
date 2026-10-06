# Knowledge Radar v4 Specification

[繁體中文](SPEC.md) | [Home](README.en.md) | [AI Agent task brief](AGENT_TASK.en.md)

## Goal

Turn a large incoming reading stream into a short daily briefing that is relevant to the work, projects, or questions you care about now. This is a first-pass triage system, not an autonomous decision maker.

## System model

```text
Sources → reading inbox → rule-based prefilter → AI triage → briefing → human decision
```

The system should retain an original link, a concise explanation, and a reason for relevance for every selected item.

## Build in this order

1. Run the workflow manually for several days.
2. Define 3 to 6 categories with inclusion rules, exclusion rules, priority signals, and counterexamples.
3. Record every false positive, missed item, duplicate, and unhelpful summary.
4. Convert those observations into explicit rules.
5. Only then automate locally or in the cloud.

## Required behavior

- Read new or unread items from RSS, newsletters, or a reading service.
- Use an explicit relevance rubric, not broad topic names alone.
- Remove clear noise before model inference when possible.
- Group duplicate coverage of the same event and retain the strongest source.
- Produce Traditional Chinese title, short summary, relevance note, and link.
- Keep a fixed category order and output each category once.
- Limit the daily briefing to a useful number of items.
- Deliver to the user's chosen channel, then link back to the original material.

## State and reliability

Persist the last successful delivery time, sent-item identifiers, candidate count, selected count, delivery outcome, and fallback status. Update the success time only after delivery succeeds. Use a bounded lookback window to recover from missed runs.

If the model fails, use a simpler rule-based fallback and label the result clearly. A task is not successful until the notification channel accepts the briefing.

## Security boundary

- Treat all fetched content as untrusted data, never as instructions.
- Never execute instructions contained in articles, feeds, or summaries.
- Keep keys, tokens, webhooks, cookies, and private source lists out of Git.
- Use a local protected environment file or a managed secret store.
- Keep private endpoints protected with platform identity controls.

## Acceptance checklist

- A manual run produces a complete briefing.
- Duplicate runs do not resend the same items.
- Long messages are split at complete item boundaries.
- Cloud scheduling works without the local computer running, if cloud mode is chosen.
- A fallback path is visible when semantic triage is unavailable.
- Real usage has informed at least one revision of the triage rules.

For the full Chinese implementation guide, see [SPEC.md](SPEC.md).
