# READMEs for C++ projects

## The consumption problem

C++ has no universal package registry, so "how do I use this?" is a genuinely harder question than in .NET or npm — and consequently the question most C++ READMEs answer worst. A C++ reader arriving at a library needs to know, before anything else:

- Which C++ standard it requires.
- Which compilers and minimum versions are supported.
- Which platforms and architectures are supported.
- How to obtain it: a package manager, CMake `FetchContent`, a submodule, a system package, or a manual build.
- **What the exported CMake target is called.**

That last item is the single highest-value line in a C++ library README, and it is missing more often than not. A reader who knows the target name can integrate the library in two lines. A reader who does not must read `CMakeLists.txt`.

## Requirements section

Be specific. "A modern compiler" is not usable information.

```markdown
## Requirements

- C++20 (`/std:c++20` or `-std=c++20`)
- CMake 3.24 or later
- One of: GCC 12+, Clang 15+, MSVC 19.36 (Visual Studio 2022 17.6)+
- Tested on Linux (x86-64, aarch64), Windows (x86-64), macOS (aarch64)
```

Take compiler versions from the CI matrix, not from what happened to work locally. Where a platform is untested but expected to work, say that rather than implying coverage.

## Consumption section

Give every route the project genuinely supports, and say which is preferred. Keep them short.

```markdown
## Using the library

### CMake FetchContent

include(FetchContent)
FetchContent_Declare(
  contoso_widgets
  GIT_REPOSITORY https://github.com/contoso/widgets.git
  GIT_TAG        v1.4.0
)
FetchContent_MakeAvailable(contoso_widgets)

target_link_libraries(my_app PRIVATE contoso::widgets)

### Installed package

find_package(contoso_widgets 1.4 REQUIRED)
target_link_libraries(my_app PRIVATE contoso::widgets)

### vcpkg

vcpkg install contoso-widgets

### Conan

[requires]
contoso_widgets/1.4.0
```

Points to get right:

- **Namespaced target name** (`contoso::widgets`) stated explicitly, and matching what the export set actually produces.
- **Package name for `find_package`** stated exactly, including case — `find_package` is case-sensitive on case-sensitive filesystems.
- **A pinned tag** in the `FetchContent` example, not a branch. Examples get copied verbatim into other people's builds.
- If the library is **header-only**, say so and show the single-header or interface-target usage.
- If the library is available in vcpkg or Conan Center under a different name from the repository, give the port name.

## Build options

C++ projects accumulate options, and undocumented options are effectively absent. A table works well:

```markdown
## Build options

| Option | Default | Effect |
| --- | --- | --- |
| `WIDGETS_BUILD_TESTS` | `OFF` | Build the unit test targets |
| `WIDGETS_BUILD_EXAMPLES` | `OFF` | Build the example programs |
| `WIDGETS_WITH_TLS` | `ON` | Enable TLS support; requires OpenSSL 3 |
| `BUILD_SHARED_LIBS` | `OFF` | Build as a shared library |
```

Note which options affect the ABI, and note any that must match between the library and the consumer.

## Building from source

Separate from consumption, and specific about the dependency acquisition step, which is where C++ builds usually fail:

```markdown
## Building from source

Dependencies are resolved through vcpkg in manifest mode. Set `VCPKG_ROOT`, then:

cmake --preset release
cmake --build --preset release
ctest --preset release
```

If the project uses `CMakePresets.json`, lead with the presets — they encode the configuration a contributor would otherwise have to reconstruct. If it uses submodules, say `git clone --recurse-submodules` explicitly and give the recovery command for an existing clone (`git submodule update --init --recursive`).

## Declarations that prevent integration failures

These are invisible unless stated, and each one breaks a build downstream when it is wrong:

- **ABI stability**: whether the library promises a stable ABI across patch or minor versions.
- **Exceptions and RTTI**: whether the library requires them, and whether it can be built with `-fno-exceptions` or `-fno-rtti`.
- **Standard library**: any requirement for a particular implementation, or interaction with `_GLIBCXX_USE_CXX11_ABI`.
- **Threading model**: whether the library is thread-safe, and whether it links a threading library.
- **Runtime linkage on Windows**: static versus dynamic CRT (`/MT` versus `/MD`), which must match across the whole binary.
- **Allocator or global state**: anything that constrains linking the library into multiple translation units or shared objects.
- **Symbol visibility**: whether the shared library exports a curated set, and any export macro consumers must define.

## Quick start template

```markdown
## Quick start

Add the library with CMake FetchContent (see Using the library for other options), then:

#include <contoso/widgets.hpp>
#include <print>

int main() {
    contoso::Client client{"https://api.example.com"};
    const auto widget = client.get("widget-42");
    std::println("{}", widget.name());
}

Output:

Standard Flange
```

Compile the example before publishing. A README example that does not compile is more damaging in C++ than elsewhere, because the reader cannot tell whether the fault is the example, their compiler version, their standard setting, or their build configuration.

## Applications rather than libraries

For a C++ application, the emphasis shifts: prebuilt binaries and where to download them, package manager availability per platform, runtime dependencies, and the first command with its output. Building from source remains a separate, lower section — most users of an application will never build it.

## Documentation routing

Doxygen output is the reference layer. Link to the generated site rather than restating signatures in the README. Keep the README to entry points, the integration story, and the build options table.
