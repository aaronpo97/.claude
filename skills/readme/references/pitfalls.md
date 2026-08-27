# README failure modes

Each entry gives the symptom, the cost, and the fix. Use this when reviewing or repairing an existing README; work down the list and note which apply.

**Contents**
- [A. Content that is absent](#a-content-that-is-absent)
- [B. Content that is wrong](#b-content-that-is-wrong)
- [C. Content that is unusable](#c-content-that-is-unusable)
- [D. Structural and rendering defects](#d-structural-and-rendering-defects)
- [E. Language defects](#e-language-defects)
- [F. Maintenance defects](#f-maintenance-defects)

---

## A. Content that is absent

### A1. No statement of purpose
**Symptom:** The README explains what the project *is* ("a Rust crate", "a .NET library") and how to install it, but never what problem it solves or when to reach for it.
**Cost:** The reader cannot decide whether to keep reading. The empirical study of 4,226 README sections by Prana et al. (2019) found "What" and "How" content very common while purpose information was frequently missing — this is the single most widespread gap in the ecosystem.
**Fix:** Two to four sentences directly after the title: the problem, who it is for, and how it differs from the obvious alternative. Name the alternative.

### A2. No status signal
**Symptom:** Nothing indicates whether the project is experimental, stable, in maintenance, or abandoned.
**Cost:** Prospective users must infer maturity from commit dates and issue counts. Cautious ones simply leave; incautious ones adopt an abandoned dependency.
**Fix:** A short Status section. State maturity, whether the API is stable, what the support policy is, and — if the project is unmaintained — say so plainly at the top. An honest "not actively maintained; forks welcome" is a service to everyone.

### A3. Unstated prerequisites
**Symptom:** Instructions begin with a build command and assume the toolchain, SDK, platform, package manager, account, or environment variable is already in place.
**Cost:** This is the "obvious to me" README, and its cause is the curse of knowledge — the cognitive bias identified by Camerer, Loewenstein and Weber (1989), under which well-informed people cannot suppress what they know when judging what others know. What is ambient after six months of development is invisible to a first-time visitor.
**Fix:** A Requirements section with concrete versions. Then re-read the procedure asking, at each step, "what must already be true for this to work?"

### A4. No route to help
**Symptom:** No issue tracker link, no discussion forum, no security contact, no maintainer.
**Cost:** Questions arrive in the wrong places or not at all; vulnerabilities go unreported.
**Fix:** A Support section: where to ask, where to report bugs, how to report security issues privately, and expected response norms.

### A5. Missing or ambiguous licence
**Symptom:** No licence named, or a licence named in the README that does not match the `LICENSE` file.
**Cost:** Legally unusable in most commercial contexts. Corporate scanners reject it automatically.
**Fix:** Name the SPDX identifier, link to the file, and make the manifest metadata agree.

## B. Content that is wrong

### B1. Fabricated or unverified commands
**Symptom:** Install lines, flags or API calls that were composed from plausible patterns rather than read from the repository.
**Cost:** Total loss of trust on first failure, and the reader has no way to tell which of the remaining instructions are also invented.
**Fix:** Trace every command to a manifest, CI workflow, or preset file. Where the fact is unavailable, leave a visible marker rather than a confident guess.

### B2. Stale instructions
**Symptom:** The install command references a renamed package; the build step references a removed script; the minimum version predates the current dependency floor.
**Cost:** Identical to fabrication from the reader's point of view.
**Fix:** Re-verify against the current build files at review time. Where possible, have CI execute the README's commands so that drift breaks the build.

### B3. Examples using an API that no longer exists
**Symptom:** The Quick start calls methods that were renamed two major versions ago.
**Cost:** Worse than a broken install command, because it fails at compile time in the reader's own project.
**Fix:** Extract examples into a compiled, tested sample project and reference them, or at minimum compile them by hand before publishing.

### B4. Version claims without evidence
**Symptom:** "Works with Node 18+" when CI only tests 20 and 22, or "supports .NET Standard 2.0" when the project multi-targets something else.
**Fix:** State exactly what is tested. If a version is supported but untested, say that.

## C. Content that is unusable

### C1. Install and build merged
**Symptom:** A single "Getting started" section that tells a consumer to clone the repository, initialise submodules, and configure a build.
**Cost:** Consumers conclude the project is heavyweight and leave. It is a particularly common defect in C++ projects, where the consumption story is genuinely harder and so gets skipped.
**Fix:** Two sections. **Installation** — the one command a consumer needs. **Building from source** — everything a contributor needs, placed lower down.

### C2. Wall of text
**Symptom:** Long paragraphs, no headings, no code blocks in the first screen.
**Cost:** Readers scan rather than read. Unstructured prose is skipped entirely, so the content might as well not exist.
**Fix:** Headings every few paragraphs, a code block early, lists for anything enumerable, and ruthless deletion of preamble.

### C3. Quick start that does not start anything
**Symptom:** The first example is a configuration fragment, an architecture diagram, or an abstract type signature.
**Cost:** The reader has no evidence the project works and no sense of what using it feels like.
**Fix:** The first code block should be the smallest complete thing that produces visible output, with that output shown.

### C4. The novel
**Symptom:** A README carrying the full API reference, the architecture rationale, the changelog and the roadmap.
**Cost:** The important material is diluted, and every one of those documents drifts independently.
**Fix:** Keep the README as a routing document. Move depth into `docs/`, a site, or a wiki, and link.

### C5. Screenshots and demos that carry the whole explanation
**Symptom:** An animated demo instead of a text description of what the tool does.
**Cost:** Unusable to screen-reader users, unindexable, and it rots silently when the interface changes.
**Fix:** Keep the visual, add the text. Every image needs alt text and a surrounding sentence stating what it shows.

### C6. Badge inflation
**Symptom:** Fifteen badges, several of which are decorative or vanity metrics.
**Cost:** Badges are supposed to be a health signal at a glance; past a handful they become noise and push real content below the fold.
**Fix:** Three to five. Build status, current version, licence, and coverage cover most needs. Every badge should answer a question a visitor would genuinely ask.

## D. Structural and rendering defects

### D1. Links that break where the README is rendered
**Symptom:** Relative links and images that work on GitHub and break on npmjs.com, nuget.org or crates.io.
**Cost:** The package landing page — often the primary discovery surface — appears broken.
**Detail:** GitHub recommends relative links inside a repository and rewrites them for the current branch, which is why they work there and why maintainers do not notice the problem. Registries render the same file without repository context. Relative images have a long history of failing on npmjs.com, and NuGet requires absolute image URLs in a packed README.
**Fix:** If the README ships in a package, use absolute URLs for images and for anything not packed alongside it. Otherwise prefer relative links. If both, transform at pack time.

### D2. Broken heading hierarchy
**Symptom:** Multiple H1s, or H2 followed by H4.
**Cost:** The generated table of contents and screen-reader navigation both become incoherent.
**Fix:** One H1, then descend one level at a time.

### D3. A hand-maintained table of contents
**Symptom:** A manual anchor list that no longer matches the headings.
**Cost:** Broken anchors, plus maintenance work with no reward.
**Fix:** On GitHub, rely on the automatically generated outline. Keep a manual contents list only for very long files rendered somewhere without that feature — and then verify the anchors.

### D4. Template scaffolding left in place
**Symptom:** `TODO`, `Your Project Name Here`, `<!-- describe your project -->`, or an unedited generator badge.
**Cost:** Signals abandonment immediately, and undermines everything else on the page.
**Fix:** Search for placeholder patterns before publishing.

### D5. Assuming one operating system
**Symptom:** Only POSIX shell commands, or paths in only one separator convention, with no note about the others.
**Fix:** State which platforms are supported. Where commands differ, give both, or give the cross-platform form.

## E. Language defects

### E1. Marketing register
**Symptom:** "Blazing fast", "seamless", "revolutionary", "powerful and flexible".
**Cost:** Carries no information and reads as compensation for a weak project.
**Fix:** Replace each claim with the number, constraint or capability that makes it true, or delete it.

### E2. "Simply" and "just"
**Symptom:** "Simply run the migration and everything just works."
**Cost:** When it does not work, the reader concludes the fault is theirs. This is a documented usability failure, not a stylistic preference.
**Fix:** Delete the word. The sentence is always at least as good without it.

### E3. Passive and hedged instructions
**Symptom:** "It is recommended that the cache be cleared periodically."
**Fix:** "Clear the cache after every deployment."

### E4. Inconsistent terminology
**Symptom:** The same concept called a job, a task, a run and an execution across four sections.
**Cost:** The reader spends effort determining whether these are four things.
**Fix:** One term per concept, matching the code and the CLI.

### E5. Undefined jargon and unexpanded acronyms
**Fix:** Expand on first use; link project-specific terms to a definition.

## F. Maintenance defects

### F1. Duplicated content
**Symptom:** The README restates the changelog, the contributing guide, or the API reference.
**Cost:** Every copy drifts, and readers cannot tell which is authoritative.
**Fix:** One home per fact; link to it.

### F2. No ownership
**Symptom:** Nobody is named as responsible, particularly in internal repositories.
**Cost:** Questions have no destination; the repository becomes unmaintainable by anyone but its author.
**Fix:** Name maintainers or an owning team, with a contact route.

### F3. Documentation not reviewed with code
**Symptom:** The README is edited only when someone complains.
**Fix:** Add a documentation item to the pull-request checklist, and treat a behaviour change with no README change as an incomplete change.
