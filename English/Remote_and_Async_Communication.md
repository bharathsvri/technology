# Remote and Asynchronous Communication

Distributed teams rely on **writing** more than **speaking**. Clear async messages reduce timezone friction and preserve decisions.

## Principles
1. **One topic per thread** (email/Slack/ticket)—split unrelated questions.
2. **BLUF**: Bottom Line Up Front—state the decision or ask first, context after.
3. **Actionable close**: who does what by when (`@owner by Tue 5pm IST`).
4. **Link, don’t attach** when possible (docs, tickets, dashboards) to avoid version sprawl.

## Slack / Teams Habits
- Use **threads** to prevent channel flooding.
- **Status + next step** in stand-up channels instead of “done”.
- Reserve `@channel` / `@here` for true incidents or urgent org-wide asks.

## Email When Remote
- **Subject lines** carry the request: `[Action] Renew TLS cert for api.example.com`.
- Assume the reader is on mobile: short paragraphs, numbered lists.
- Paste **errors as text** (or gist link), not only screenshots—others can search and copy.

## Time Zones
- State deadlines in **UTC** or **both** local and UTC when coordinating globally.
- Share **working hours** in your profile; respect “focus blocks”.

## Documentation-First Culture
- Decisions in **ADR** (Architecture Decision Record) or wiki with date and alternatives considered.
- Update the **ticket** when scope changes—chat alone is not the system of record.

## Related Topics
- `Client_Communication_Guide.md`
- `Meetings_and_Status_Updates.md`
