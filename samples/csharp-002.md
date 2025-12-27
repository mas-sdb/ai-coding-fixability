# C# Ecosystem Evaluation Sample

**AI Model:** GPT-5.2  
**Date:** 2024-01-15

---
## Information to Provide as Input
- Language name 
    - C#
- Language version 
    - C# 7.3
- Major libraries used 
    - Entity Framework 6
- Runtime environment
    - Windows (.NET Framework 4.8.1)
---

## Evaluation Results

### ① Community Consistency

**AI Mis-inference Points:**
- Pulled by general knowledge that ".NET (formerly .NET Core) is now mainstream," easily ignores .NET Framework 4.8.1 assumptions (App.config/Web.config, IIS, GAC, AppDomain, Binding Redirect, etc.)
- Entity Framework knowledge easily mixes EF6 and EF Core, assuming EF Core design/APIs/migration procedures
- Language features: easily suggests C# 8+ features (nullable reference types, async streams, switch expressions, records) as given

**Recommended Mitigations:**
- Explicitly state upfront: ".NET Framework 4.8.1 / C# 7.3 / EF6 strictly enforced (.NET / EF Core answers prohibited)"
- Provide fixed information on app type (WinForms/WPF/ASP.NET MVC/WebForms/Windows Service) and database type (SQL Server, etc.)
- Document project-adopted patterns (DI presence/absence, Repository pattern presence, DbContext lifecycle policy)

---

### ② Documentation Consistency

**AI Mis-inference Points:**
- EF6 official documentation and EF Core official documentation (docs.microsoft.com) easily cross-contaminate during search/recall
- .NET Framework configuration docs (connection strings, providers, Binding Redirect, machine.config influence) tend to be old versions or fragmented
- C# 7.3 spec/feature boundaries easily overwritten by latest C# explanations

**Recommended Mitigations:**
- Fix authoritative references ("EF6 documentation," ".NET Framework 4.8 configuration," "C# 7.3 constraints") and establish rule to exclude EF Core articles
- Specify documentation reference assumptions: "configuration assumes App.config/Web.config," "SDK-style csproj explanations prohibited"
- List "available language features (prohibited/permitted)" in AGENT.md

---

### ③ Practice Consistency

**AI Mis-inference Points:**
- EF6 practical styles vary per project (EDMX/Database First, Code First, manual SQL, Repository/UoW presence, Lazy Loading handling)
- DbContext scope (per-screen/per-request/per-method) and tracking policy (AsNoTracking usage) easily decided with generic assumptions ignoring project context
- Legacy .NET Framework apps often use synchronous APIs, but AI unifies with "always async recommended," conflicting with UI threads and existing design

**Recommended Mitigations:**
- Specify EF usage style (EDMX/Code First, Migration operation presence, LazyLoading/ProxyCreation policy, tracking policy)
- Document DbContext creation/disposal responsibility, transaction boundaries, exception handling (retry/logging) as project standards
- Fix implementation guidelines: "don't break existing calling conventions (sync/async)," "UI app ConfigureAwait policy"

---

### ④ Dependency Stability

**AI Mis-inference Points:**
- EF6 is stable, but surrounding dependencies (SQL client, DI container, JSON, Logging) easily get "latest recommended" mixed in, breaking .NET Framework compatibility and version alignment
- .NET Framework prone to Binding Redirect and dependency DLL conflicts, but AI underestimates this operation (config updates, package pinning)
- NuGet operation differs greatly between packages.config/PackageReference, but AI easily misunderstands assumptions

**Recommended Mitigations:**
- Document "major dependency fixed versions," "NuGet approach (packages.config or PackageReference)," "Binding Redirect operation rules"
- Constrain new library introductions to "only those with clear .NET Framework 4.8.1 support" (require compatibility verification when suggesting)
- Prohibit suggestions replacing EF6 peripherals (providers, SQL client) with EF Core equivalents

---

### ⑤ API Consistency

**AI Mis-inference Points:**
- EF6 and EF Core API names are similar, easily mixing incorrect extension methods or configuration APIs (model building, Include behavior, Migrations/Scaffold differences)
- Ignores .NET Framework BCL API vs .NET (formerly Core) API differences (non-existent types/methods, behavior differences) and suggests calls
- SQL client confusion (System.Data.SqlClient vs Microsoft.Data.SqlClient) causes misaligned connection/exception/encryption settings assumptions

**Recommended Mitigations:**
- Establish checkpoint: "use EF6 API set only," "EF Core-specific APIs/extensions not allowed"
- Always include ".NET Framework 4.8.1 target" in prompts and establish rule requiring "suggested APIs must verify existence in that framework"
- Clarify DB connection library adoption (which to use) and prohibit mixed suggestions

---

### ⑥ Ecosystem Consistency

**AI Mis-inference Points:**
- Build/project format (old csproj, MSBuild, AssemblyInfo, app.config, Web.config) easily discussed assuming SDK-style
- Execution hosts (IIS/Windows Service/Desktop) have vastly different configuration/deployment/permissions, but AI tends toward single "modern" procedure (containers, Kestrel)
- When using Windows-specific features (certificate store, event log, performance counters, COM), answers assume cross-platform

**Recommended Mitigations:**
- Provide app type and host environment (IIS, Service, or Desktop) as fixed information
- Specify project format (old csproj/SDK-style distinction, .config presence, CI MSBuild version)
- Declare Windows-dependent feature usage (event log usage presence) to narrow inference scope

---

### ⑦ Static Semantic Service

**AI Mis-inference Points:**
- C# 7.3 has no nullable reference types; AI suggests "NRT-based safety measures" (design/annotations/warning operations don't exist)
- Easily assumes advanced static analysis flows assuming Roslyn/Analyzer without confirming current build or IDE configuration
- EF6 queries (LINQ to Entities) easily equated with "C# LINQ," statically misjudging translatability constraints (server-side translation)

**Recommended Mitigations:**
- Document static constraints: "C# 7.3: no NRT, latest syntax prohibited"
- List adopted Analyzer/Style rules (.editorconfig, FxCop/StyleCop, Roslynator) and don't assume non-existent ones
- Establish project convention that EF6 LINQ "has translation constraints" and require translation possibility/SQL generation verification for query changes

---

### ⑧ Runtime Semantic Service

**AI Mis-inference Points:**
- EF6 runtime behavior (Lazy Loading, Proxy, Tracking, connection/transaction, exception types) varies greatly by configuration, but AI easily explains with default assumptions
- .NET Framework assembly resolution depends on Binding Redirect; easily underestimates runtime-only load exceptions
- Sync/async mixing, UI threads, ASP.NET synchronization context—runtime deadlock/hang landmines easily missed with generic assumptions

**Recommended Mitigations:**
- Fix and document EF6 runtime settings policy (LazyLoadingEnabled/ProxyCreationEnabled/AutoDetectChangesEnabled, etc.)
- Establish procedure to "always verify" runtime exception observation means (logging infrastructure, EF logs/SQL logs, event log, health checks)
- Define async policy (await usage in UI/ASP.NET, synchronous blocking prohibition rules) as project standard

---

### ⑨ Core Semantic Consistency

**AI Mis-inference Points:**
- C# language spec is relatively consistent, but "C# new features" and "target BCL/.NET Framework features" are separate; AI easily assumes "if language can write it = runtime supports it"
- .NET Framework backward compatibility/legacy mechanisms (AppDomain, CAS remnants, config files, old crypto/communication settings) mean modern .NET conventions don't directly apply
- EF6 itself prioritizes long-term compatibility, but suggestions easily apply latest ORM trends (EF Core design philosophy) directly

**Recommended Mitigations:**
- Fix all three as a set every time: "language spec (C# 7.3)," "runtime platform (.NET Framework 4.8.1)," "ORM (EF6)" and establish rule not to reinterpret any as latest
- Document "prohibited replacements" (e.g., EF6→EF Core, .NET Framework→.NET assumption replacement, SDK-style conversion assumptions)
- Provide design policy prioritizing compatibility/constraints (minimize changes, respect existing configuration, respect deployment form) as basic guideline to AI

---

## Summary

This evaluation demonstrates challenges when working with legacy .NET Framework infrastructure. The gap between modern .NET (formerly .NET Core) community knowledge and .NET Framework 4.8.1 reality creates significant AI mis-inference risks.

**Key Takeaway:**  
Legacy .NET Framework projects require extremely explicit version constraints. AI's training heavily favors modern .NET/EF Core patterns, making it critical to repeatedly state ".NET Framework 4.8.1 / C# 7.3 / EF6 only" and prohibit modern alternatives in every interaction.
