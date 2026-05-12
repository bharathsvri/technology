# Code Review and Pull Request Etiquette

## Goals of a Pull Request (PR)
- Share **intent** and **risk** with reviewers.
- Produce **mergeable** history that future readers can bisect and understand.

## Author Checklist
- **Small PRs**: easier review than thousand-line dumps.
- **Description template**: context, approach, screenshots for UI, links to tickets.
- **Self-review** first: comment on non-obvious lines proactively.
- **Tests** for behavior changes; note what you did **not** test and why.
- **CI green** before requesting review.

## Reviewer Habits
- **Ask questions** before asserting wrongness—assume good intent.
- Separate **blocking** issues (correctness, security, data loss) from **nits** (style).
- Prefer **suggestions** with concrete snippets using the review tool.
- **Approve with comments** when minor follow-ups can be addressed without another round.

## Tone
- “**Consider** extracting X for readability” beats “This is bad.”
- If you do not understand something, say so—**confusion is a valid review signal**.

## Responding to Feedback
- **Resolve** threads when addressed; push commits or explain why not.
- **Re-request** review after substantial changes.

## Related Topics
- `Remote_and_Async_Communication.md`
- `Meetings_and_Status_Updates.md`
