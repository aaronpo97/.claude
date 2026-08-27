# READMEs for C# and .NET projects

## Where the README is rendered

A .NET README typically appears in three places, each with different constraints:

1. **The repository front page** on GitHub or Azure DevOps — relative links work.
2. **The package landing page on nuget.org** — the README is embedded in the `.nupkg` and rendered without repository context.
3. **The README tab in the Visual Studio NuGet package manager**, for packages that embed one.

## Embedding the README in the package

NuGet does not pick up `README.md` automatically. Two things are required: a property naming the file, and an item that actually packs it.

```xml
<PropertyGroup>
  <PackageReadmeFile>README.md</PackageReadmeFile>
</PropertyGroup>

<ItemGroup>
  <None Include="README.md" Pack="true" PackagePath="\" />
</ItemGroup>
```

Notes that catch people out:

- The file must be packed to the package root; `PackagePath="\"` does that.
- **Images must use absolute URLs.** Relative image paths render on GitHub and break on nuget.org, because the images are not inside the package.
- The same applies to links to files that are not packed — `CONTRIBUTING.md`, `LICENSE`, `docs/`. Use absolute repository URLs if the README ships in the package.
- Setting `PackageReadmeFile` outside the `.csproj` (in a shared `.props` file) has historically been unreliable; verify the packed output with `dotnet pack` and inspect the `.nupkg` before relying on it.

Related metadata worth setting alongside it, because nuget.org surfaces all of it: `PackageId`, `Description`, `Authors`, `PackageTags`, `PackageLicenseExpression`, `PackageIcon`, `PackageProjectUrl`, `RepositoryUrl`.

## Sections that matter more in .NET

### Target frameworks

State the target frameworks explicitly, taken from `<TargetFramework>` or `<TargetFrameworks>`. Readers need to know before they try:

```markdown
## Requirements

- .NET 8.0 or later, or any runtime supporting .NET Standard 2.0
- Multi-targets `net8.0`, `net9.0`, `netstandard2.0`
```

Do not translate a target framework moniker into a marketing name and lose the precision. `net472` and `netstandard2.0` mean specific things.

### Version and support policy

.NET consumers care about the support relationship between the package and the runtime. State:

- Which .NET versions are tested in CI, not merely which ones might work.
- Whether the package follows semantic versioning.
- What happens when a .NET version reaches end of support.

### Installation

Give the consumer path first, and give the common variants:

```markdown
## Installation

dotnet add package Contoso.Widgets

Or via the Package Manager Console:

Install-Package Contoso.Widgets
```

Do not open with `git clone`. Cloning belongs in "Building from source".

### Capability flags worth declaring

These determine whether the package can be used at all in some projects, and are invisible without being stated:

- **Nullable reference types**: whether the public API is annotated.
- **Trimming and AOT**: whether the assembly is trim-compatible and Native AOT-compatible.
- **Thread safety**: which types are safe for concurrent use.
- **Async support**: whether the API is async-first, sync-first, or both, and whether `ConfigureAwait(false)` is applied internally.
- **Analyzers or source generators**: if the package ships them, say what they emit and how to suppress them.
- **Platform constraints**: Windows-only APIs, P/Invoke dependencies, native assets.

### Build, test and run commands

Take these from the CI workflow rather than from habit:

```markdown
## Building from source

Requires the .NET 9 SDK.

dotnet restore
dotnet build -c Release
dotnet test
```

If the repository uses Central Package Management (`Directory.Packages.props`), `Directory.Build.props`, custom MSBuild targets, or a build orchestrator such as NUKE or Cake, say so — a contributor who edits a `.csproj` version reference and finds it ignored will not work out why unaided.

### Configuration

For libraries with an options pattern, show registration rather than describing it:

```csharp
services.AddContosoWidgets(options =>
{
    options.Endpoint = new Uri("https://api.example.com");
    options.Timeout = TimeSpan.FromSeconds(30);
});
```

Then a table of options, defaults and effects. Defaults especially — an undocumented default is a production incident waiting to happen.

## Quick start template

```markdown
## Quick start

Install the package:

dotnet add package Contoso.Widgets

Register the client:

using Contoso.Widgets;

var client = new WidgetClient("https://api.example.com");
var widget = await client.GetAsync("widget-42");

Console.WriteLine(widget.Name);
// Output: Standard Flange
```

Every identifier in that block should exist in the shipped API. Compile it before publishing.

## Documentation routing

XML documentation comments are the reference layer for .NET, and the README should not duplicate them. Link to the generated API documentation site (DocFX, Sandcastle, or the docs host) rather than restating signatures. Keep the README's API section to entry points only — the three or four types a new user touches first.
