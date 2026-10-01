---
name: scrum-spike-document
description: Create Scrum/Agile spike documents (time-boxed research or investigation items). Use whenever the user asks for a spike, spike story, spike ticket, technical or functional spike, research spike, proof-of-concept write-up, or wants to turn an open question, risk or un-estimable backlog item into a structured investigation, or to record the findings of one, even if they do not use the word "spike".
---

# Scrum Spike Document

Produce a clear, business-like spike document that turns uncertainty into a bounded, time-boxed investigation with a defined output, and, once the work is done, records the findings and the decision.

## Background (use to inform the document, not to lecture the user)

- A spike is a time-boxed research or investigation activity whose output is knowledge (a decision, finding, prototype or proof of concept), not production-ready functionality.
- The term originates in Extreme Programming (XP) and is attributed to Kent Beck. The Scrum Guide does not define spikes; Scrum teams adopt them as a practice. The Scaled Agile Framework (SAFe) treats spikes as a type of Enabler Story.
- Spikes are commonly classified as:
  - **Technical** – evaluating how to implement something (technology choice, feasibility, performance, integration, build-versus-buy).
  - **Functional** – clarifying what should be built (user behaviour, requirements, workflow, interaction design).
- Typical purposes: reduce technical or functional risk, make a story estimable, assess feasibility, or familiarise the team with a new technology or domain.

When citing any of these points in a document, state them as practice or convention, not as rules of the Scrum framework.

## Language and tone

- Use standard International English (e.g. "prioritise", "organisation", "behaviour") unless the user specifies otherwise.
- Use a concise, business-like tone. Prefer short declarative sentences and bulleted criteria.
- Do not invent facts about the user's system, organisation or data. Where information is missing, insert a clearly marked placeholder such as `[TBC: owner]` and list it under Open Questions.

## Workflow

1. **Determine the mode.**
   - *Planning*: the spike has not yet been run; produce the proposal sections.
   - *Findings*: the spike is complete; produce or update the Findings, Recommendation and Follow-up sections.
   - If unclear, infer from the user's wording ("we need to investigate…" = planning; "here's what we found…" = findings).
2. **Extract the essentials** from the conversation or attached material: the question, why it matters, related backlog item(s), constraints and the time-box. Ask at most one or two concise questions only if the central question or the time-box cannot be inferred; otherwise proceed with placeholders.
3. **Sharpen the question.** Rewrite vague goals ("research performance") into one answerable question ("Can service X sustain 500 requests per second on current infrastructure?"). If there are several unrelated questions, recommend separate spikes.
4. **Set the time-box.** Express it in hours or days, not story points, unless the team's convention differs. A spike should fit within a single sprint. If the user proposes a longer effort, flag it and suggest splitting.
5. **Write verifiable acceptance criteria** focused on the knowledge or artefact delivered, not on shipping a feature.
6. **Produce the document** using the template below. Omit sections that are genuinely not applicable rather than leaving them empty, except Open Questions and Assumptions, which should always be present.
7. **Check before delivering**: the question is a single answerable question; the time-box is explicit; each acceptance criterion is checkable; the expected output is named; follow-up actions are identified (findings mode).

## Template

```markdown
# Spike: [Concise question being answered]

| Field | Value |
|---|---|
| Spike ID | [e.g. PROJ-123 or TBC] |
| Type | Technical / Functional |
| Related item(s) | [Epic / feature / story IDs] |
| Owner | [Name or TBC] |
| Time-box | [e.g. 2 days / 16 hours] |
| Sprint | [Sprint name or number] |
| Status | Proposed / In progress / Complete / Abandoned |
| Date | [YYYY-MM-DD] |

## 1. Background and Context
Why this spike is needed: the uncertainty, risk or blocker, and which backlog item cannot be estimated or committed to without it.

## 2. Question(s) to Answer
- Primary question (one sentence, answerable).
- Supporting questions (optional, maximum three).

## 3. Scope
**In scope:** …
**Out of scope:** …

## 4. Approach
Planned activities (e.g. prototype, benchmark, vendor documentation review, stakeholder interviews, user testing) and any options to be compared.

## 5. Evaluation Criteria
Criteria used to judge options (e.g. performance, cost, security, maintainability, licence, team capability), with weighting if relevant.

## 6. Acceptance Criteria (Definition of Done for the Spike)
- [ ] The primary question is answered with supporting evidence.
- [ ] Findings and a recommendation are documented and shared with the team.
- [ ] Follow-up backlog items are created or refined and can be estimated.
- [ ] [Spike-specific criterion, e.g. prototype demonstrates X]

## 7. Expected Output
The artefact(s) to be delivered: decision record, findings report, proof of concept, architecture sketch, refined stories, estimate.

## 8. Assumptions, Constraints and Dependencies
- …

## 9. Risks
Risk | Likelihood | Impact | Mitigation

## 10. Open Questions
- …

---
<!-- Complete the following after the spike -->

## 11. Findings
Summary of what was learnt, with evidence (data, benchmark results, links, references). Distinguish verified facts from observations and opinions, and cite sources.

## 12. Options Considered
Option | Pros | Cons | Assessment against evaluation criteria

## 13. Recommendation and Decision
Recommended option, rationale, and the decision taken (and by whom), or note that a decision is pending.

## 14. Follow-up Actions
- New or refined backlog items, with estimates where possible.
- Further spikes, if the question was not fully answered and why.

## 15. Time-box Outcome
Time spent versus time-box. If the time-box was exhausted before the question was answered, state what remains unknown and the recommended next step (stop, re-scope or new spike); do not silently extend.

## 16. References
Links to code, prototypes, documents and external sources used.
```

## Output format

- Default to the session's document format for shareable writing (for example a Claude Docs document) when available; otherwise produce Markdown.
- Produce a Word (.docx) file, a ticket body for Jira/Azure DevOps, or another format only when the user asks for it. For a ticket, condense to: Title, Type, Time-box, Question, Acceptance Criteria, Expected Output and Related items.
- After delivering, give a one- or two-sentence summary and list any placeholders the user needs to complete.

## Example

**Input:** "We need to know whether we can move our PDF generation to a serverless function before we commit to the reporting epic. Two days max."

**Output (abridged):**
- Title: *Spike: Can PDF generation run reliably as a serverless function within our cost and latency targets?*
- Type: Technical | Time-box: 2 days | Related: Reporting epic [TBC: ID]
- Acceptance criteria: prototype generates a representative 20-page report; cold-start and execution latency measured against target [TBC]; monthly cost estimated at current volume; recommendation documented; reporting stories refined for estimation.
- Open questions: latency target, current monthly volume, epic ID.
