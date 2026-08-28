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
- Omit emojis from code or output unless explicitly asked for.

## Change presentation
- Reference code as `file_path:line_number` rather than pasting large blocks back.
- Show diffs or edits, not whole-file dumps, unless the whole file is genuinely needed for context.
