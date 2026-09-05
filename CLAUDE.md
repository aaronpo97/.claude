# Output guidelines

## Default format
- Lead with the outcome, not the process.
- Limit responses to 5 lines of prose by default (excluding code blocks).
- Expand beyond 5 lines only when the user asks for more detail, or the task is inherently multi-part (e.g. distinct options to weigh, a multi-step plan needing review, a research or investigation task).

## Commits
- Write a single sentence in the imperative mood (e.g. "Fix null check on login" not "Fixed" or "Fixes").
- Omit "Co-Authored-By" and any other session or tool attribution.

## Code style
- Add comments only to explain non-obvious *why* (a hidden constraint, workaround, or surprising behaviour). Never restate what the code already says.
- No negative framing in documentation (comments, doc comments, summaries). Describe what something is/does, not what it isn't/doesn't do.
- Omit emojis from code or output unless explicitly asked for.

## Verification
- Before reporting a task complete, run the relevant tests/build/lint if available; state clearly if you couldn't verify.
- Before implementing or debugging an integration with an unfamiliar library, plugin, or API (e.g. "does X support Y"), perform a web search to confirm feasibility first. Don't burn effort reverse-engineering or trial-and-error-ing a workaround for something a two-minute search would have told you isn't supported.

## Clarifying questions
- If a request is ambiguous and a wrong guess would be costly to undo, ask before proceeding. Otherwise pick a sensible default and note the assumption.

## Change presentation
- Reference code as `file_path:line_number` rather than pasting large blocks back.
- Show diffs or edits, not whole-file dumps, unless the whole file is genuinely needed for context.
