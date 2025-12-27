# C# Ecosystem Evaluation Sample (Minimal Context)

**AI Model:** GPT-5.2  
**Date:** 2024-01-15

---
## Information to Provide as Input
- Language name 
    - C#
---

## Evaluation Results

### ① Community Consistency

**AI Mis-inference Points:**
- ".NET Framework (legacy)" and ".NET (current: .NET 6+)" contexts easily cross-contaminate (ASP.NET, configuration, dependency resolution, build/execution forms are different)
- Even within ASP.NET Core, many schools exist (MVC / Razor Pages / Minimal APIs / gRPC / Worker); AI tends to answer with patterns from different schools
- DI, logging, configuration, HTTP, ORM (EF Core/Dapper) have "standard-ish defaults" but field preferences vary widely

**Recommended Mitigations:**
- Specify target in use (e.g., .NET 8, ASP.NET Core Minimal APIs, EF Core, xUnit) and "unadopted schools" in AGENT.md
- Fix "adopted libraries and patterns (layer structure, responsibility division)" for DI/config/logging/HTTP/ORM in text
- For legacy assets, document "acceptable scope for referencing .NET Framework knowledge"

---

### ② Documentation Consistency

**AI Mis-inference Points:**
- Microsoft Learn is latest-oriented, but search hits on old Docs/blogs/StackOverflow mix in old recommendations (e.g., old configuration methods, old APIs)
- Same features split across "language (C#) explanations" and "runtime/library (.NET)" pages on different sites, causing version assumption misalignment
- `System.Text.Json` vs Newtonsoft.Json—effectively two major streams; documentation references easily fluctuate

**Recommended Mitigations:**
- Specify "authoritative references allowed" (Microsoft Learn for target version, adopted library official docs, internal ADRs)
- Prepare brief bullet lists per major topic (HTTP, JSON, DI, Logging, Configuration, Testing, ORM) defining "this project's truth"
- Explicitly prohibit/specify exception conditions for unadopted old technologies (e.g., Web.config, WCF, old ASP.NET)

---

### ③ Practice Consistency

**AI Mis-inference Points:**
- Coding conventions (naming, nullable, exception policy, async policy, LINQ usage) vary significantly per team
- "Clean Architecture / Onion / Vertical Slice / traditional 3-tier"—architectural school differences are large; AI mixes different structures
- Testing also varies: xUnit/NUnit/MSTest, mocking approaches, integration test organization

**Recommended Mitigations:**
- Document `.editorconfig` and static analysis (Roslyn analyzers) assumptions, fixing nullable/warning treatment/formatting policy
- Fix directory structure, layer boundaries, naming conventions, exception/logging policy, async usage rules in AGENT.md
- Document test policy (unit/integration boundaries, frameworks used, mocking approach)

---

### ④ Dependency Stability

**AI Mis-inference Points:**
- NuGet is mature, but surrounding packages (OpenAPI, Auth, Cloud SDK, Observability, frontend integration) update rapidly and encounter breaking changes easily
- Transitive dependency differences cause "works locally but breaks in CI"; AI easily misidentifies cause
- `.NET` target framework updates change compatibility and API availability

**Recommended Mitigations:**
- Fix dependency management policy (central management `Directory.Packages.props` adoption, update frequency, acceptable ranges, compatibility checks)
- Prepare lock operation (`packages.lock.json`) or Renovate/Dependabot rules and instruct AI "don't arbitrarily do major updates"
- Specify target framework (e.g., net8.0) and support scope; if multiple TFMs, document reasons and constraints

---

### ⑤ API Consistency

**AI Mis-inference Points:**
- BCL (standard library) has heavy overloading; similar-named APIs abound, easily selecting subtly different calls (sync/async, culture, encoding)
- HTTP/JSON "recommended usage" has evolved; AI easily mixes old recommendations (e.g., inappropriate `HttpClient` reuse policy)
- `DateTime`/`DateTimeOffset`, `Guid`, `CultureInfo`, `CancellationToken`—many APIs prone to misuse in edge cases

**Recommended Mitigations:**
- Declare project-standard adopted APIs (what for HTTP, which for JSON, which for date-time, cancellation required, etc.)
- Document wrapper/common utility presence and fix rules like "don't call BCL directly, use common layer" in text
- Keep important design decisions (e.g., date-time UTC unified, don't leak exceptions to domain) briefly as ADRs

---

### ⑥ Ecosystem Consistency

**AI Mis-inference Points:**
- Toolchain (dotnet CLI / MSBuild / Visual Studio / Rider / CI) behavior differences (artifacts, analyzers, SDK resolution) emerge easily
- Windows-assumption knowledge (paths, certificates, IIS) mixing with cross-platform assumptions leads AI to environment-dependent solutions
- Source Generators, AOT, trimming adoption status drastically changes build/execution constraints

**Recommended Mitigations:**
- Document `global.json` SDK pinning operation presence, CI OS/SDK, build commands
- Document execution environment (Linux container, Windows service, Azure) and constraints (AOT/trimming presence, certificate operation)
- Instruct that CLI/CI-reproducible procedures are "correct," not IDE-dependent steps

---

### ⑦ Static Semantic Service

**AI Mis-inference Points:**
- C# has strong type information, but `dynamic`, Reflection, DI containers, Source Generators add "invisible wiring"; AI can't trace statically and misinterprets
- Nullable reference types enabled/disabled changes semantics; AI easily misunderstands null safety
- With Analyzer/StyleCop/custom rules, AI easily suggests "compiles but violates conventions"

**Recommended Mitigations:**
- Document Nullable settings (enabled, which projects enabled) and warning treatment (WarningsAsErrors, etc.)
- List analyzers used and important rules (exceptions: permissible cases) in AGENT.md
- For heavy Reflection/DI/Generator usage areas, explain "entry points (registration locations) and conventions" in text to provide exploration clues

---

### ⑧ Runtime Semantic Service

**AI Mis-inference Points:**
- Exception types and timing (especially async/await, I/O, LINQ deferred execution) easily misread for behavior
- Sync/async context, thread pool, cancellation, timeout, retry—operational semantics involvement leads to mis-inference
- Culture/timezone/encoding/filesystem differences—environment-dependent runtime factors easily overlooked

**Recommended Mitigations:**
- Define "standard policy" for exception/retry/timeout/cancellation (where to catch, how to log, how to return)
- Document execution environment assumptions (culture fixed, UTC, container, Kestrel configuration)
- For critical features, establish integration tests/runtime verification as truth and instruct AI "don't make static assertions" (prioritize reproduction steps and log observation locations)

---

### ⑨ Core Semantic Consistency

**AI Mis-inference Points:**
- C# continuously evolves (records, pattern matching, required, primary constructors); language version differences change "can write/can't write" and "recommended writing style"
- .NET also changes BCL/APIs and default behaviors per version; AI easily suggests assuming different versions
- Backward compatibility is strong, but "old writing styles still work" causes AI to mix rather than align with latest policy

**Recommended Mitigations:**
- Specify `TargetFramework` and `LangVersion` (fixed or latest-tracking which) and declare adopted language feature scope
- Prohibit-list "features/notations not used in this project" (e.g., old APIs, specific syntax, specific compatibility modes)
- Keep compatibility-related decisions (e.g., libraries maintain broad TFM, apps fix to latest) as brief policies

---

## Summary

This evaluation demonstrates C#/.NET ecosystem complexity when given minimal context. The platform's continuous evolution (.NET Framework → .NET Core → .NET 5+) and rich ecosystem create multiple valid but incompatible approaches.

**Key Takeaway:**  
C# projects require explicit declarations about .NET version (.NET Framework vs modern .NET), framework choices (ASP.NET Core style), and language version (C# 7.3 vs 12). Without these constraints, AI easily mixes incompatible patterns across different generations and architectural styles.
