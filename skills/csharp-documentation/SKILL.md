---
name: csharp-documentation
description: Write, review, or repair XML documentation comments (///) in C#. Use this skill whenever the user asks to document C# code, add or improve summaries on classes/methods/properties, prepare a library for NuGet or DocFX publication, fix CS1591/CS1573 build warnings, or asks Claude to write C# that will be consumed by other developers. Also use it when reviewing C# and the existing comments are noisy, stale, or merely restate the identifier — even if the user did not use the word "documentation".
---

# C# Documentation

Documentation earns its place by carrying information the signature does not. The reader already sees the name, the types, the accessibility and the nullability. Anything that repeats those is noise, and noise is not neutral: it hides the two lines that mattered and it rots silently when the code changes.

Default to fewer, denser lines.

## The deletion test

Before writing any line, ask: **if I deleted this, what would the reader no longer know?** If the answer is "nothing", do not write it.

```csharp
// Fails the test — the signature already said all of this.
/// <summary>Gets or sets the customer identifier.</summary>
public int CustomerId { get; set; }

// Passes — the constraint and the sentinel are invisible from the signature.
/// <summary>Gets or sets the customer identifier. Zero indicates an unsaved customer.</summary>
public int CustomerId { get; set; }
```

For genuinely self-evident members, one plain `<summary>` line is the correct and complete answer. Do not pad it with an empty `<remarks>`, a restated `<returns>`, or `<param>` tags that echo the parameter names.

## What to document

Cover the **public and protected surface** — the contract other people code against. Internal and private members are documented only where the logic is genuinely non-obvious; documenting them wholesale exposes implementation detail and doubles the maintenance burden.

Spend the words on what the reader cannot deduce:

| Worth saying | Because the signature cannot express it |
|---|---|
| Null, empty and sentinel behaviour | `string?` says a null may be returned, not what it means |
| Units, ranges, precision | `double timeout` — seconds or milliseconds? |
| Side effects and mutation | Whether the caller's collection is modified |
| Ownership and disposal | Who disposes the stream that was passed in |
| Thread safety | Safe for concurrent readers? |
| Ordering and stability guarantees | Whether results are deterministic |
| Performance characteristics | O(n²), or a network call behind an innocuous name |
| Exceptions the caller can act on | Trigger conditions, not just the type |
| Rationale for surprising behaviour | Why the obvious approach was rejected |

Everything else is usually already on screen.

## House phrasing

.NET has settled conventions. Follow them — consistency is what makes generated reference documentation readable in bulk.

| Construct | Opening |
|---|---|
| Method | Third-person verb: `Calculates…`, `Sends…`, `Returns…` |
| Read/write property | `Gets or sets…` |
| Read-only property | `Gets…` |
| Boolean property | `Gets or sets a value indicating whether…` |
| Constructor | `Initializes a new instance of the <see cref="Foo"/> class.` |
| Exception type | `The exception that is thrown when…` |
| `<exception>` | `Thrown when…` |

Write complete sentences ending in full stops. Keep `<summary>` to one sentence; move anything longer into `<remarks>`.

## Tags

The compiler validates the starred tags and warns when they are wrong, which makes them worth using precisely.

**Core:** `<summary>`, `<remarks>`, `<returns>`, `<param>`*, `<exception>`*, `<value>`, `<typeparam>`*

**References:** `<see cref="…"/>`* for code, `<see href="…">text</see>` for URLs (`cref` will not produce a clickable link to a web page), `<see langword="null"/>` for keywords, `<seealso>`* for a See Also section, `<paramref name="…"/>` and `<typeparamref name="…"/>` to point at parameters from prose.

**Reuse:** `<inheritdoc/>` on overrides and interface implementations rather than copy-and-paste. Note that Visual Studio infers inherited documentation in IntelliSense automatically, but the compiler does **not** write it into the generated XML file — so for a published library the tag must be present explicitly. `<include file='…' path='…'/>` moves bulky prose into a side file; the .NET runtime repository uses this heavily.

**Formatting:** `<c>` inline code, `<code>` blocks, `<example>`, `<list type="bullet|number|table">`, `<para>` (double-spaced), `<br/>` (single-spaced). Escape literal angle brackets as `&lt;` and `&gt;`.

Documentation comments cannot be applied to a namespace.

## Templates

Method — only the lines that carry weight:

```csharp
/// <summary>Reserves stock for an order, decrementing available quantity.</summary>
/// <param name="sku">Stock keeping unit. Matching is case-insensitive.</param>
/// <param name="quantity">Units to reserve. Must be greater than zero.</param>
/// <returns>
/// A reservation handle, or <see langword="null"/> if insufficient stock exists.
/// </returns>
/// <exception cref="ArgumentOutOfRangeException">
/// Thrown when <paramref name="quantity"/> is zero or negative.
/// </exception>
/// <remarks>Reservations expire after 15 minutes unless confirmed.</remarks>
public Reservation? Reserve(string sku, int quantity)
```

Asynchronous method — describe the result, not the mechanics of `Task`:

```csharp
/// <summary>Retrieves the customer, querying the upstream service on cache miss.</summary>
/// <param name="cancellationToken">Token to observe while awaiting completion.</param>
/// <returns>
/// A task whose result is the customer, or <see langword="null"/> if no customer
/// exists with that identifier.
/// </returns>
public Task<Customer?> GetAsync(int id, CancellationToken cancellationToken = default)
```

Generic type:

```csharp
/// <summary>A bounded queue that discards the oldest item when full.</summary>
/// <typeparam name="T">The element type. Reference and value types are both supported.</typeparam>
/// <remarks>Not thread-safe. Wrap access in a lock for concurrent use.</remarks>
public sealed class RingBuffer<T>
```

Override:

```csharp
/// <inheritdoc/>
public override void Dispose()
```

Where an override changes the contract meaningfully, use `<inheritdoc/>` for the inherited text and add a `<remarks>` describing only the difference.

## Anti-patterns

| Instead of | Write |
|---|---|
| `/// <summary>Gets the name.</summary>` on `Name` | Nothing, or the constraint that matters |
| `/// <param name="user">The user.</param>` | Drop it, or state what makes a user valid here |
| `/// <returns>A bool.</returns>` | What `true` actually means |
| `/// <exception cref="Exception">Thrown on error.</exception>` | The specific type and its trigger condition |
| Restating the implementation step by step | The contract; the body is already visible |
| `<remarks>` containing a restated `<summary>` | Delete it |
| Commented-out code inside `<code>` | A minimal, compiling example |

Stale documentation is worse than none, because the reader trusts it. When behaviour changes, amend the comment in the same commit.

## Project configuration

```xml
<PropertyGroup>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
</PropertyGroup>
```

This produces the `.xml` file alongside the assembly — required for IntelliSense in consuming projects, for NuGet packages, and for Swashbuckle/OpenAPI descriptions — and enables **CS1591** (missing comment on a publicly visible member).

Related warnings worth leaving on: CS1570 (malformed XML), CS1571 (duplicate `<param>`), CS1572 (`<param>` for a parameter that does not exist), CS1573 (parameter with no `<param>` tag).

For a project not yet fully documented, suppress selectively rather than abandoning the build signal:

```xml
<NoWarn>$(NoWarn);CS1591</NoWarn>
```

Then re-enable per file with `#pragma warning restore CS1591` as areas are completed. Place shared settings in `Directory.Build.props` so every project inherits them. Source generators frequently trip CS1591 on generated code, which is the usual reason a team suppresses it.

Publishing: DocFX and Sandcastle consume the XML file and turn `cref` references into hyperlinks. DocFX also renders Markdown placed inside a `CDATA` section within `<remarks>`.

## Review checklist

1. Does every public and protected member have a `<summary>` that survives the deletion test?
2. Is each `<summary>` one sentence, in the conventional voice for its construct?
3. Are null, empty, unit, range and sentinel behaviours stated wherever they exist?
4. Do documented exceptions state the trigger condition, and are undocumented-but-thrown ones covered?
5. Are `cref` references resolving — no compiler warnings on the documentation itself?
6. Are overrides using `<inheritdoc/>` rather than duplicated prose?
7. Has anything been written that the reader could already see? Delete it.
