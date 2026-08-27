# READMEs for TypeScript and JavaScript projects

## Where the README is rendered

`README.md` is published to npm by default and becomes the package page on npmjs.com. It also renders on the repository forge. The two environments differ in one important way, covered next.

## The relative link problem on npmjs.com

npmjs.com renders the README without access to the repository's files. Historically, relative image paths have failed to render there, and relative links to files such as `docs/guide.md` or `LICENSE` resolve incorrectly or 404. The registry attempts to rewrite relative links using the `repository` field in `package.json`, but the rewriting has known gaps — particularly for monorepo packages published from a subdirectory, and for links rewritten against the default branch rather than the published tag.

Practical guidance:

- Set `repository` in `package.json`, including `directory` for monorepo packages. This is the input the rewriter relies on.
- **Use absolute URLs for images.** Point at `raw.githubusercontent.com` or a CDN. Accept that absolute URLs break if the repository or default branch is renamed, and prefer a tagged path over a branch path where stability matters.
- Use absolute URLs for links to files not included in the published package.
- Verify by running `npm pack` and inspecting the tarball, or by checking the package page immediately after a pre-release publish.

## Sections that matter more in the npm ecosystem

### Installation, with the package managers people actually use

Give one primary command and the variants. Readers using pnpm or Bun should not have to translate.

```markdown
## Installation

npm install @scope/package
# pnpm add @scope/package
# yarn add @scope/package
# bun add @scope/package
```

For a CLI, show the `npx` form as well, since many users will never install it:

```markdown
npx @scope/tool init
```

### Module format

This is the highest-value paragraph in a modern JavaScript README, and it is usually missing. State plainly:

- Whether the package is ESM-only, CommonJS-only, or dual.
- What the `exports` map provides, including any subpath exports.
- Whether types are bundled, and where the type entry point is.
- Whether the package has side effects (`sideEffects` in `package.json`), which determines tree-shaking behaviour.

```markdown
## Module format

ESM only. Requires Node.js 20 or later, or a bundler that resolves the `exports` field.
CommonJS `require()` is not supported; use dynamic `import()` from CJS.
Types are bundled — no `@types/` package is needed.
```

An ESM-only package that does not say so generates a stream of "ERR_REQUIRE_ESM" issues.

### Runtime and TypeScript support policy

- Which Node.js versions are tested in CI. Take this from the workflow matrix, not from `engines` alone, and note where the two disagree.
- Which browsers or other runtimes (Deno, Bun, Cloudflare Workers, React Native) are supported.
- Minimum TypeScript version, if the types use recent syntax.
- Whether `strict` mode is required for the types to behave as documented.

### Peer dependencies

If the package has peer dependencies, state them and their supported ranges near the top of Installation. A peer dependency discovered at install time as a warning is a poor first experience.

### Type-checked examples

TypeScript examples that do not compile are the most common defect in TypeScript READMEs, because nothing checks them.

- Write examples against the real exported names.
- Include the import statement — readers copy the whole block.
- Show inferred types in comments where inference is the selling point.
- Consider extracting examples into an `examples/` directory that is compiled in CI, and keeping the README block in sync with it.

```typescript
import { createClient } from '@scope/package';

const client = createClient({ endpoint: 'https://api.example.com' });

const widget = await client.get('widget-42');
//    ^? Widget

console.log(widget.name);
// => "Standard Flange"
```

### Framework compatibility

If the package is used with React, Vue, Svelte, Next.js, or similar, say which versions are supported and whether there are framework-specific entry points, server-component constraints, or bundler configuration requirements.

### Bundle size

If size matters for the package's audience, state the minified and gzipped size and how it was measured, or link to a size-tracking service. A number with a method beats an adjective.

## Monorepo considerations

For a monorepo, the root README is a map: what each package is, and a link to its own README. Each published package needs its own README, because that is what npm renders for it. Do not point the package's `readme` at the root file — consumers of one package get the wrong page.

Set `repository.directory` per package so link rewriting and "Repository" links resolve correctly.

## Quick start template

```markdown
## Quick start

npm install @scope/package

import { createClient } from '@scope/package';

const client = createClient({ endpoint: process.env.API_URL });
const widgets = await client.list({ limit: 10 });

console.log(widgets.map((w) => w.name));
// => [ 'Standard Flange', 'Reinforced Flange' ]
```

## Documentation routing

Keep the README to entry points and the shortest working example. Generated API documentation (TypeDoc or similar) belongs on a documentation site, linked from a Documentation section. Do not restate type signatures in prose — they drift immediately, and the types themselves are the authoritative reference.
