# Writing Effective Technical Documentation

Good docs reduce **onboarding time**, **incident** confusion, and **repeated questions** in chat.

## Audience First
- **Runbook readers** want commands, verification steps, rollback.
- **Architect readers** want constraints, trade-offs, diagrams.
- **API consumers** want examples, error shapes, and versioning rules.

## Structure
1. **Title + one-line purpose**
2. **Prerequisites** (access, tools, feature flags)
3. **Procedure** numbered steps
4. **Verification** (“how you know it worked”)
5. **Rollback / failure modes**
6. **Owners and last reviewed date**

## Style
- Prefer **active voice** and **present tense** for procedures.
- Use **exact** UI paths, property keys, and **full** URLs in internal wikis when stable.
- Keep **examples copy-pasteable**; scrub secrets.

## Diagrams
- One diagram beats a long paragraph for **data flow** and **trust boundaries**.
- Regenerate or version diagrams with the doc when systems change.

## Maintenance
- Tie docs to **tickets** when large changes land; add a **“deprecated”** banner rather than deleting history abruptly.

## Related Topics
- `Remote_and_Async_Communication.md`
- `Code_Review_and_Pull_Request_Etiquette.md`
