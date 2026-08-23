# The technical documentation register

**Contents**
1. [What "register" means here](#1-what-register-means-here)
2. [Person, mood and tense](#2-person-mood-and-tense)
3. [Voice](#3-voice)
4. [Sentence construction](#4-sentence-construction)
5. [Modality: must, should, may](#5-modality-must-should-may)
6. [Terminology discipline](#6-terminology-discipline)
7. [Information ordering](#7-information-ordering)
8. [Headings](#8-headings)
9. [Lists and tables](#9-lists-and-tables)
10. [Code examples as prose](#10-code-examples-as-prose)
11. [Writing for a global audience](#11-writing-for-a-global-audience)
12. [Accessibility](#12-accessibility)
13. [Words and phrases to replace](#13-words-and-phrases-to-replace)
14. [Worked revisions](#14-worked-revisions)

---

## 1. What "register" means here

Register is the variety of language selected for a particular situation and audience. Technical documentation occupies a specific register: **direct, impersonal about the writer, personal about the reader, present-tense, and unadorned**. It is neither academic prose nor marketing copy nor conversational chat, and borrowing from any of those three is the usual way a README goes wrong.

Three bodies of practice converge on much the same advice, which is why it can be relied on:

- **The Google developer documentation style guide** (developers.google.com/style), made public in 2017, prescribes second person, present tense, active voice, and the serial comma, with a tone that is conversational and professional rather than formal or dry.
- **ASD-STE100 Simplified Technical English**, a controlled natural language originating with European aerospace in the 1980s and now an international standard (Issue 9, January 2025), consists of roughly 53 writing rules plus a dictionary of approximately 900 approved words, each restricted to a single meaning and part of speech. It exists because aircraft maintenance instructions are read by people who cannot ask the author what was meant.
- **Diátaxis** (diataxis.fr) separates documentation into tutorials, how-to guides, reference and explanation, and holds that each requires a different way of writing.

A README does not need to be fully STE-conformant. It does benefit from STE's core discipline: one meaning per word, active voice, simple tenses, one instruction per sentence, short sentences, and no words dropped to save space.

## 2. Person, mood and tense

| Element | Rule | Example |
| --- | --- | --- |
| Person | Second person for the reader. Avoid "we" except for genuine project-team statements. | "You can override the default with `--config`." |
| Mood | Imperative for every step in a procedure. | "Run `npm install`." not "You'll want to run `npm install`." |
| Tense | Present simple, including for describing what happens after an action. | "The build writes artefacts to `dist/`." not "The build will write..." |
| Aspect | Simple aspect. Avoid present perfect and continuous forms. | "The parser reads the file." not "The parser has been reading the file." |

Mood by section:

- **Quick start, Installation, Building, Testing** — imperative. These are procedures.
- **Why this exists, Status, Explanation** — declarative, present tense, third person about the software.
- **Configuration, API summaries** — declarative and terse; noun phrase plus behaviour.

Do not use "please" in instructions. Politeness in a procedure adds words without adding meaning.

## 3. Voice

Use active voice. Passive voice hides the actor, and in a procedure the reader needs to know who or what performs the action — themselves, the tool, the compiler, or the server.

| Avoid | Prefer |
| --- | --- |
| The configuration is loaded at startup. | The service loads the configuration at startup. |
| The tests can be run with `dotnet test`. | Run the tests with `dotnet test`. |
| It is recommended that the token be rotated. | Rotate the token every 90 days. |
| Errors are logged to stderr. | The CLI writes errors to stderr. |

Passive voice is acceptable in descriptive text when the actor is genuinely unknown or irrelevant: "The package is published to nuget.org on every tagged release." Even there, an active alternative is usually available and usually better.

## 4. Sentence construction

- **One instruction per sentence.** If a step contains "and then", it is two steps.
- **Length ceilings**: roughly 20 words for procedural sentences, 25 for descriptive ones. These are STE limits and work well as a discipline rather than a hard rule.
- **Do not drop words to shorten a sentence.** Removing articles, subjects or verbs produces telegraphic text that reads as terse but parses ambiguously. "Set flag in config" is worse than "Set the `verbose` flag in the configuration file."
- **Limit noun clusters to three words.** "Default cache invalidation strategy configuration value" is unparseable; write "the configuration value for the default cache-invalidation strategy", or rename the concept.
- **Avoid ambiguous pronouns.** "This" and "it" at the start of a sentence usually refer to something the reader has to reconstruct. Repeat the noun.
- **One paragraph, one idea.** Three to four sentences is generally enough; a README paragraph longer than five sentences will be skipped.

## 5. Modality: must, should, may

Be deliberate about obligation. The distinctions from RFC 2119 are useful even outside a specification:

| Word | Meaning |
| --- | --- |
| **must** / **must not** | Absolute requirement; ignoring it breaks something |
| **should** / **should not** | Recommended; valid reasons may exist to deviate |
| **may** / **can** | Genuinely optional |

Use "can" for capability ("You can pass multiple paths") and "may" for permission ("You may omit the flag in development"). Avoid stacked hedges — "you might possibly want to consider perhaps enabling" states nothing. If it matters, say so; if it does not, delete it.

## 6. Terminology discipline

Fix one term per concept and use it everywhere — in the README, the CLI help text, the API, and the error messages. Silently switching between "job", "task", "run" and "execution" forces the reader to work out whether four things exist or one.

- Do not use the same word as both noun and verb in close proximity.
- Do not use a word that also names something else in the domain.
- Introduce any project-specific term on first use, in one sentence, then link to a fuller explanation.
- Expand every acronym on first use, unless it is more widely known than its expansion (HTTP, JSON, API).
- Keep the term matching the code. If the class is `Workspace`, the README says "workspace", not "project".

## 7. Information ordering

**Condition before instruction.** The reader needs to know whether a step applies before reading the step:

> To publish a pre-release build, add the `--prerelease` flag.

not

> Add the `--prerelease` flag to publish a pre-release build.

**Most important first, within every unit.** Section order, paragraph order and sentence order all follow this. The reader who stops after one screen should still have the essentials.

**Prerequisites before procedures.** Anything that will make a step fail — a required runtime, an environment variable, an account, a platform limitation — is stated before the first command, not discovered halfway through.

**Warnings before the action they concern.** A caution placed after the destructive command is decoration.

## 8. Headings

- **Sentence case**: "Get started", not "Get Started".
- **One H1** (the project name), then H2 for sections and H3 for subsections. Do not skip levels; the heading hierarchy is what generates the rendered table of contents and what screen readers navigate by.
- **Task sections take verb phrases**: "Configure logging", "Build from source".
- **Reference sections take noun phrases**: "Configuration options", "Supported platforms".
- Headings are navigation, not decoration. A reader scanning only the headings should be able to reconstruct the shape of the document.
- Avoid decorative emoji in headings if consistency and screen-reader output matter; if the project uses them, use them uniformly rather than on some headings only.

## 9. Lists and tables

- Use a vertical list whenever text contains three or more parallel items or a sequence of steps.
- **Numbered lists for sequences**, bulleted lists for unordered sets.
- **Keep list items parallel**: all imperative clauses, or all noun phrases, never a mixture.
- Do not bury a step inside a paragraph. If it is a step, it is a list item.
- Use a table when every item shares the same attributes — options with defaults, platforms with support status, versions with end-of-life dates. Tables are hostile on narrow screens, so keep them to a few columns.

## 10. Code examples as prose

Code blocks are read more carefully than the text around them, so they carry the register too.

- **Always tag the language** on the fence, for syntax highlighting and for tooling.
- **Complete and runnable.** No `...` standing in for required lines. If the example genuinely needs elision, make the omission explicit with a comment.
- **No shell prompt characters** (`$`, `>`, `PS>`) at the start of copyable commands; they break copy-paste and add nothing.
- **Separate command from output.** Put expected output in its own block, or clearly comment it.
- **Use real names from the actual API.** Placeholder identifiers such as `foo` and `doSomething()` are the clearest signal that the author did not check the code.
- **Mark substitutions clearly**: `<your-api-key>` with a sentence saying where to obtain it.
- **Keep the example minimal but honest.** Omit error handling only if you say so; do not present an unsafe pattern as the recommended one.

## 11. Writing for a global audience

Most readers of English technical documentation are not native English speakers. This is the reason STE exists, and it applies to any project with an international user base.

- Avoid idiom, metaphor, humour and cultural references — they translate poorly and often not at all.
- Avoid phrasal verbs where a single verb exists: "start" rather than "kick off", "remove" rather than "get rid of", "continue" rather than "carry on".
- Avoid Latin abbreviations. Write "for example" instead of "e.g.", "that is" instead of "i.e.", "and so on" instead of "etc.".
- Avoid negative constructions, especially double negatives: "the flag is optional", not "the flag is not required".
- Avoid directional language such as "above" and "below" in procedures; rendering order varies. Link to the section by name.
- Avoid contractions in warnings and requirements, where "don't" is easier to misread than "do not".
- Keep dates unambiguous: 2026-08-04 or 4 August 2026, never 04/08/26.
- Give units, and give them consistently.

## 12. Accessibility

- **Alt text on every image** that describes what the image conveys, not that it is an image. "Architecture diagram: the CLI writes to the queue, workers read from it" beats "screenshot".
- **Descriptive link text.** "See the [configuration reference]" rather than "click [here]". Link text is read out of context by screen readers and indexed out of context by search engines.
- **Do not rely on colour alone** to convey status in badges or diagrams.
- **Do not use ASCII art or emoji as structural elements** — they are read aloud character by character.
- **Keep tables simple**: no merged cells, no nested tables, header row present.

## 13. Words and phrases to replace

| Avoid | Use |
| --- | --- |
| simply, just, easily, obviously, of course | (delete — if it were obvious the reader would not be reading) |
| blazing fast, lightning quick, buttery smooth | a measured number with the conditions it was measured under |
| powerful, robust, seamless, elegant, modern | the specific capability that makes the claim true |
| utilise, leverage | use |
| in order to | to |
| at this point in time | now |
| prior to | before |
| in the event that | if |
| a number of, several | the number |
| should you wish to | to |
| please note that | (delete, and state the fact) |
| it is recommended that you | (imperative: "Rotate the key every 90 days") |
| the user | you |
| kill, abort, execute (of processes, where a plainer word exists) | stop, cancel, run |
| sanity check | check, validate |
| dummy value | placeholder, sample value |
| master/slave | primary/replica, controller/worker |
| whitelist / blacklist | allowlist / denylist |
| he, she (generic) | they, or "the user" recast to second person |

The last four are inclusive-language substitutions now standard in the major style guides; the rest are precision substitutions.

## 14. Worked revisions

**Vague purpose → concrete purpose**

> Before: A powerful and flexible library for working with data.
>
> After: Parses fixed-width mainframe export files into typed .NET records, without loading the whole file into memory.

**Buried prerequisite → stated prerequisite**

> Before: Run `make build` to compile the project. (Note: requires the Vulkan SDK.)
>
> After: **Requires:** Vulkan SDK 1.3 or later, CMake 3.24 or later, and a C++20 compiler.
> Compile the project: `cmake --preset release && cmake --build --preset release`

**Passive procedure → imperative procedure**

> Before: The configuration file should be copied to the home directory and then it can be edited as required.
>
> After: 1. Copy `config.example.toml` to `~/.config/app/config.toml`. 2. Set `api_key` to the key from your account page.

**Multi-instruction sentence → one instruction per sentence**

> Before: Install the dependencies and then build the solution and run the tests to verify everything works.
>
> After: 1. Restore the dependencies: `dotnet restore`. 2. Build the solution: `dotnet build`. 3. Run the tests: `dotnet test`.

**Hedged obligation → clear obligation**

> Before: You may want to consider possibly setting the connection timeout, as the default might not be suitable in some cases.
>
> After: Set `ConnectionTimeout` explicitly for production workloads. The default of 15 seconds is tuned for local development.

**Marketing claim → verifiable claim**

> Before: Blazing fast serialisation with zero overhead.
>
> After: Serialises a 10 MB payload in 42 ms on the benchmark machine, against 310 ms for the built-in serialiser. See [benchmarks] for the method and hardware.
