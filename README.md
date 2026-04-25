# dnSpy Unity Mono for Unity 2022.3.62f2

_Custom dnSpy-compatible Mono build for Unity 2022.3.62f2 / Cities: Skylines II_

Based on the original dnSpy Unity mono work by Neoshrimp and later community updates.  
This repository is a cleaned and focused version for **Unity 2022.3.62f2 only**.

---

## Purpose

This repository contains the patched Unity mono source tree and Visual Studio solution needed to build a dnSpy/debugger-compatible version of:

- `mono-2.0-bdwgc.dll`
- the matching `.pdb`

Primary use cases:

- debugging Mono/.NET code in **Unity 2022.3.62f2**
- debugging games that use **MonoBleedingEdge**
- especially useful for **Cities: Skylines II**

---

## Target version

This repo is intended for:

- **Unity 2022.3.62f2**
- MonoBleedingEdge / `.NET 4.x`
- `mono-2.0-bdwgc.dll`

Patched Unity mono commit used for this version:

- **2022.3.62f2-mbe** → `e2f0981f5c01bfa427f6aef9a9fa5cce45f51a55`

---

## What is included

This repository is intentionally reduced to the files needed for **Unity 2022.3.62f2**.

Included:

- patched source tree for `unity-2022.3.62f2`
- Visual Studio solution for 2022.x
- patched build files for current toolchains
- support for building `mono-2.0-bdwgc.dll`
- README and helper files

Not included:

- older Unity version trees
- unrelated legacy solutions
- prebuilt release archives for every Unity version

---

## Requirements

Recommended environment:

- **Visual Studio 2022**
- Desktop development with C++
- Windows 10/11 SDK
- MSBuild / VC toolchain
- Git

Toolset notes:

- Unity 2022.x works with modern VS2022 toolchains
- **v143** is the expected toolset for this repository
- some original projects may still prompt for toolset upgrade when opened in Visual Studio

---

## Opening the project

Open the solution:

- `2022.3.62f2-dnSpy-Unity-mono-v2022.x.sln`

Recommended settings:

- **Configuration:** `Release`
- **Platform:** `x64`

Main projects of interest:

1. `eglib`
2. `genmdesc`
3. `libgc`
4. `libmono`
5. `libmono-dynamic`

---

## Building

Build the solution in this order if needed:

1. `eglib`
2. `genmdesc`
3. `libgc`
4. `libmono`
5. `libmono-dynamic`

Expected output location:

- `unity-2022.3.62f2\msvc\build\boehm\x64\bin\Release\mono-2.0-bdwgc.dll`
- `unity-2022.3.62f2\msvc\build\boehm\x64\bin\Release\mono-2.0-bdwgc.pdb`

---

## Notes about source fixes

While preparing this version, several project and build path issues had to be resolved due to layout differences between older dnSpy Unity mono repos and Unity 2022.3.62f2.

Typical fixes included:

- Boehm GC source/include path adjustments
- `support/libm` path restoration
- `corefx/brotli` related source path fixes
- successful rebuild of all 5 required projects
- generation of a working `mono-2.0-bdwgc.dll` for runtime replacement

This repo is meant to preserve those fixes so others do not need to repeat the same setup work.

---

## Using the built DLL in a game

Example target path for Cities: Skylines II:

- `...\MonoBleedingEdge\EmbedRuntime\mono-2.0-bdwgc.dll`

Typical replacement workflow:

1. Back up the original DLL.
2. Copy in the newly built `mono-2.0-bdwgc.dll`.
3. Copy the matching `mono-2.0-bdwgc.pdb` beside it.
4. Start the game.
5. Attach dnSpy or Visual Studio.

Important:

- make sure the game actually loads the replaced DLL
- verify the loaded module path in Process Explorer, Visual Studio, dnSpy, or PowerShell
- if symbols are loaded correctly, line-level debugging becomes possible inside the custom Mono DLL

---

## Verifying the loaded DLL

Useful checks:

- confirm file path of loaded `mono-2.0-bdwgc.dll`
- confirm the `.pdb` is loaded
- confirm module timestamp/size matches your compiled output
- confirm the game is using the target `EmbedRuntime` path you replaced

If the debugger still cannot attach, common causes are:

- wrong runtime path replaced
- symbols not loaded
- wrong Mono variant
- port conflict / reserved debugger ports
- the game is using a different process/runtime instance than expected

---

## dnSpy attach/debugging notes

Rarely, debugger connection can fail because Windows has reserved the port range used for Mono soft debugging.

You can inspect reserved TCP ranges with:

```powershell
netsh int ipv4 show excludedportrange tcp
```

Also helpful:

- inspect listening ports with `Get-NetTCPConnection`
- inspect the loaded module path with `Get-Process <process> -Module`
- verify symbol loading in dnSpy or Visual Studio

---

## Repository scope

This repository is **not** a universal Unity mono archive.  
It is a focused repo for one version only:

- **Unity 2022.3.62f2**

That keeps it:

- smaller
- easier to maintain
- easier to verify
- easier for others to reuse specifically for this version

If support for other Unity versions is needed, separate branches or separate repos are recommended.

---

## Suggested branch/release strategy

Recommended:

- keep `main` or `master` focused on `2022.3.62f2`
- optionally create a branch like `unity-2022.3.62f2`
- publish a GitHub release containing:
  - `mono-2.0-bdwgc.dll`
  - `mono-2.0-bdwgc.pdb`
  - short usage notes

---

## Credits

Original foundation and idea:

- Neoshrimp
- notsapinho
- dnSpy Unity mono community contributors

Version-specific adaptation, build fixes, cleanup, and packaging for:

- **Unity 2022.3.62f2**

---

## Commit reference

| version | git hash |
|---|---|
| 2022.3.62f2-mbe | `e2f0981f5c01bfa427f6aef9a9fa5cce45f51a55` |

---

## Disclaimer

This repository is intended for reverse engineering, debugging, modding research, and compatibility work.  
Use it at your own risk.  
Always back up original game files before replacing runtime DLLs.