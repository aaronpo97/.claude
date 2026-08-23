# README review checklist

A standalone checklist, suitable for pasting into a pull request or handing to a maintainer.

## Accuracy

- [ ] Every command traces to a manifest, CI workflow or preset file — none written from memory
- [ ] Install command matches the published package name
- [ ] Version and framework/runtime claims match the build files and the CI matrix
- [ ] Code examples use identifiers that exist in the current public API
- [ ] Examples compile and run as written
- [ ] Licence named matches the `LICENSE` file and the package metadata

## Completeness

- [ ] Purpose stated: the problem solved, and how this differs from the obvious alternative
- [ ] Status stated: maturity, API stability, support policy
- [ ] Prerequisites stated with concrete versions
- [ ] Supported platforms stated
- [ ] Route to help stated: questions, bug reports, security disclosure
- [ ] Maintainer or owning team identified

## Usability

- [ ] Installation (consumers) is separate from Building from source (contributors)
- [ ] A complete, runnable example appears within the first screen or two
- [ ] Expected output is shown where output is the point
- [ ] Configuration options list their defaults
- [ ] The file routes outwards rather than absorbing the API reference and changelog
- [ ] Badges number five or fewer and each is informative

## Rendering

- [ ] Links resolve in every environment where the file is rendered — repository, and package registry if published
- [ ] Images resolve in every environment (absolute URLs if the README ships in a package)
- [ ] One H1; heading levels descend without skipping
- [ ] Code fences are language-tagged
- [ ] No shell prompt characters that break copy-paste
- [ ] No placeholder text, template scaffolding, or unresolved `TODO`
- [ ] Images have descriptive alt text
- [ ] Link text is descriptive rather than "here"

## Language

- [ ] Second person, present tense, active voice
- [ ] Imperative mood for every instruction
- [ ] One instruction per sentence
- [ ] Conditions precede the instructions they qualify
- [ ] One term per concept, matching the code and CLI
- [ ] Acronyms expanded on first use
- [ ] No "simply", "just", "easily", "obviously"
- [ ] No unmeasured performance or quality claims
- [ ] Spelling variety consistent throughout

## Maintenance

- [ ] No content duplicated from another document that will drift
- [ ] No hand-maintained table of contents where the renderer generates one
- [ ] Generated reference material is linked, not restated
