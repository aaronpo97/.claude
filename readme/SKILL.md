---
name: readme
description: Write, review, repair, or restructure README.md files for code projects. Use this skill whenever the user mentions a README, asks to document a repository, package, service or library, is scaffolding a new project, is preparing to publish to NuGet, npm, vcpkg or Conan, or complains that nobody understands their project. Also use it when reviewing an existing README that is stale, vague, unstructured, or a wall of text, and when writing any repository front page — even if the user never says the word "README". Covers general practice plus C#/.NET, TypeScript/JavaScript and C++ specifics.
---

# README authoring

A README is the front door of a repository. GitHub surfaces it automatically when it sits in the repository root, in `docs/`, or in the hidden `.github/` directory, and package registries such as nuget.org and npmjs.com render it as the package landing page. It is usually the only document a visitor reads before deciding whether to keep going.

That makes the README a **routing document**, not the documentation. Its job is to answer five questions quickly, then hand off:

1. **What** is this?
2. **Why** does it exist, and why choose it over the alternatives?
3. **How** do I get it working, right now?
4. **What state** is it in — is it maintained, and is it safe to depend on?
5. **Where** do I go next for depth, help, or to contribute?

Empirical work on README content found that "What" and "How" material is very common, while information about a repository's **purpose and status is frequently missing** (Prana et al., *Categorizing the Content of GitHub README Files*, Empirical Software Engineering 24, 2019). Treat "Why" and "Status" as first-class sections rather than afterthoughts, because those are the ones most projects forget.

## Step 1 — Gather facts; never invent them

The fastest way to destroy trust in a README is a command that does not work. A fabricated install line, an invented flag, or a version number guessed from context is worse than no README at all, because the reader discovers the error only after wasting their time.

Read the repository before writing a single line. Every factual claim in the output must trace back to something observed:

| Claim to be made | Where to verify it |
| --- | --- |
| Project name, version, description | `*.csproj` / `Directory.Build.props`, `package.json`, `CMakeLists.txt` `project()`, `vcpkg.json` |
| Install command | Package manifest and registry name; confirm the package is actually published |
| Supported runtime/compiler versions | `TargetFramework(s)`, `engines`, `CMAKE_CXX_STANDARD`, `cmake_minimum_required`, CI matrix |
| Build and test commands | CI workflow files (`.github/workflows/*.yml`), `Makefile`, `package.json` scripts, `CMakePresets.json` |
| Dependencies and prerequisites | Lockfiles, manifests, `vcpkg.json`, `conanfile.py`, Dockerfiles |
| Public API surface used in examples | The actual exported symbols — read the source or the type definitions |
| Licence | The `LICENSE` file and the licence field in the manifest |
| Status and maturity | Version number, release history, recent commit activity, existing badges |

Where a fact cannot be established, do not paper over the gap. Either ask the user directly, or mark it explicitly:

```markdown
<!-- TODO(maintainer): confirm minimum supported Node.js version -->
```

An explicit, visible gap is honest and gets fixed. A confident guess gets copied into someone's build pipeline.

When reviewing an existing README rather than writing a new one, verify the existing claims too. Stale instructions are the second most common defect after missing ones, and they are invisible unless checked against the current build files.

## Step 2 — Identify what kind of project this is

The section set follows from the project type. Choosing the wrong shape produces a README that is technically complete and still useless.

| Type | Reader's first goal | Emphasis |
| --- | --- | --- |
| **Library / package** | Add it to their project and call it | Install, minimal working example, API entry points, version support policy |
| **Application / CLI** | Run it | Install, first command with real output, common tasks, configuration |
| **Service** | Deploy or call it | Endpoints, run locally, configuration, environment variables, operational notes |
| **Internal / team repository** | Understand ownership and how to work in it | Purpose, ownership, local setup, conventions, deployment, runbook links |
| **Monorepo root** | Find the right package | Map of packages, shared tooling, where per-package READMEs live |
| **Template / example** | Copy it | What it demonstrates, how to adapt it, what to delete |

Ask the user if the type is genuinely ambiguous. Do not ask if the repository answers the question — a `bin` entry in `package.json` or a `Program.cs` with a top-level entry point already says what this is.

## Step 3 — Structure

Use this order unless there is a reason to depart from it. Sections marked optional are omitted when they do not apply; an empty section is worse than an absent one.

```markdown
# Project name                          <- one H1, matches the repo and package name
One-line description                    <- under ~120 characters, matches the manifest description
[badges]                                <- optional, 3-5 maximum

## Why this exists                      <- the problem, and how this differs from alternatives
## Status                               <- maturity, support policy, stability guarantees
## Requirements                         <- optional; versions, platforms, toolchain
## Installation                         <- for consumers
## Quick start                          <- smallest complete example that produces visible output
## Usage                                <- optional; task-shaped how-to sections
## Configuration                        <- optional; table of options, defaults, precedence
## Documentation                        <- links out to the real documentation
## Building from source                 <- optional; separate from Installation
## Testing                              <- optional
## Contributing                         <- link to CONTRIBUTING.md, do not inline it
## Support                              <- where to ask questions, how to report bugs
## License                              <- SPDX identifier and link
```

Three structural rules matter more than the exact list:

**Separate installing from building.** A consumer wants `dotnet add package Foo`. A contributor wants the toolchain, submodules and build flags. Merging them forces every consumer to read contributor instructions and conclude the project is hard to use.

**Front-load the payoff.** The path from landing on the page to running working code should be short. Put the smallest complete example above every optional section. Readers scan rather than read; long preamble is skipped, and material below the first screen is often never seen.

**Keep the four documentation types apart.** The Diátaxis framework (diataxis.fr) distinguishes tutorials (learning by doing), how-to guides (achieving a goal), reference (lookup), and explanation (understanding). These need different writing and serve different needs, and mixing them degrades all of them. In a README: Quick start is a miniature tutorial, Usage is how-to, Configuration is reference, and "Why this exists" is explanation. Keep them in separate sections and resist letting explanation leak into the Quick start — link to it instead.

Anything that grows past a screen or two belongs in `docs/`, a wiki, or a documentation site, with the README linking to it. GitHub's own guidance is that a README should carry what is needed to get started, with longer material held elsewhere.

## Step 4 — Write it

Full guidance is in `references/language-register.md` — read it before drafting. The essentials:

- **Second person, present tense, active voice.** "The function returns a string", not "A string will be returned by the function".
- **Imperative mood for every instruction.** "Install the SDK", not "You should install the SDK" or "The SDK needs to be installed".
- **One instruction per sentence**, with a rough ceiling of 20 words for procedural text and 25 for descriptive text. These limits come from ASD-STE100, the controlled-English standard built for aerospace maintenance documentation, where a misread instruction has physical consequences.
- **One term, one meaning.** Do not alternate between "config", "settings", "options" and "parameters" for the same thing.
- **Condition before instruction.** "To enable tracing, set `TRACE=1`" — not "Set `TRACE=1` to enable tracing", which makes the reader parse an action before knowing whether it applies to them.
- **State prerequisites explicitly.** The curse of knowledge — the cognitive bias described by Camerer, Loewenstein and Weber (1989), whereby better-informed people cannot set aside what they know — is the root cause of the "obvious to me" README. Assume the reader is intelligent and unfamiliar with this project's ecosystem.
- **Every code block must be complete and runnable.** No ellipses in place of required code, no placeholder identifiers without saying what to substitute, no shell prompt characters that break copy-paste. Show real output where output is the point.
- **Prefer plain description to marketing language.** "Blazing fast", "seamless" and "revolutionary" carry no information and cost credibility. Give the number, the benchmark, or the constraint instead.

Match the project's existing spelling variety and terminology if any documentation already exists. Otherwise pick one variety and hold it consistently across the file.

## Step 5 — Verify before delivering

Work through this list; it catches the defects that survive drafting. The full catalogue with explanations is in `references/pitfalls.md`.

- [ ] Every command was traced to a build file, CI workflow or manifest — none were composed from memory.
- [ ] Install instructions are for consumers; contributor build steps are in a separate section.
- [ ] Prerequisites state concrete versions, not "a recent version".
- [ ] The Quick start example is complete, uses real API names, and shows expected output.
- [ ] Purpose and status are both present and specific.
- [ ] Links resolve in **every** place the file is rendered — see the warning below.
- [ ] Headings are sentence case, form a single H1 with no skipped levels, and describe content rather than decorate it.
- [ ] No placeholder text, no leftover template scaffolding, no `TODO` that was meant to be filled in.
- [ ] Badges are few and informative; each one answers a question a visitor would actually ask.
- [ ] The licence is named with its SPDX identifier and matches the `LICENSE` file.
- [ ] There is a stated route for getting help and reporting bugs.
- [ ] Nothing in the file duplicates information that lives elsewhere and will drift out of sync.

### The link and image trap

This one is worth calling out separately, because it is invisible until after publication.

GitHub recommends **relative** links within a repository, and rewrites them automatically for the current branch, so they survive clones and forks. Package registries do not have the repository's files: npmjs.com and nuget.org render the same README elsewhere, where relative paths break. Relative image paths have a long history of failing to render on npmjs.com, and NuGet requires images referenced from a packed README to use absolute URLs.

So: if the README ships in a published package, use absolute URLs for images and for links to files that are not inside the package. If it only ever renders on the forge, prefer relative links. If it does both, either use absolute URLs throughout or transform the README at pack time.

## Maintenance

A README rots faster than code because nothing fails when it goes stale. Reduce the surface area:

- Do not restate version numbers, dependency lists or API signatures that are generated elsewhere — link to the generated artefact.
- Do not hand-maintain a table of contents on GitHub; the rendered view generates one from the headings.
- Prefer a short README that links out over a long one that repeats other documents.
- If the project has a changelog, link to it rather than summarising release history.

## Reference files

Read the file that matches the work at hand:

- `references/language-register.md` — the technical English register: mood, person, tense, modality, sentence and paragraph construction, terminology discipline, internationalisation, accessibility, and a substitution table. Read this before drafting or editing prose.
- `references/pitfalls.md` — catalogue of README failure modes: symptom, why it costs the project, and the fix. Read this when reviewing or repairing an existing README.
- `references/csharp.md` — C# and .NET: NuGet README embedding, target frameworks, version support statements, build and test commands.
- `references/typescript.md` — TypeScript and JavaScript: npm rendering constraints, ESM/CJS and types entry points, package manager variants, runtime support policy.
- `references/cpp.md` — C++: the consumption problem, CMake target names, package managers, build options, standard and toolchain declarations.

Assets:

- `assets/README-template.md` — annotated skeleton with guidance comments to strip out.
- `assets/review-checklist.md` — standalone checklist suitable for handing to the user or pasting into a pull request.
