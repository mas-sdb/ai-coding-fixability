# Java Ecosystem Evaluation Sample (Minimal Context)

**AI Model:** GPT-5.2  
**Date:** 2024-01-15

---
## Information to Provide as Input
- Language name
    - Java
---

## Evaluation Results

### ① Community Consistency

**AI Mis-inference Points:**
- Proceeds assuming "Java = Spring," ignoring alternative ecosystems like Jakarta EE / Micronaut / Quarkus / Play
- Build culture splits between Maven / Gradle (Groovy DSL / Kotlin DSL), leading to mismatched assumptions about configuration, directory conventions, and dependency management
- Teams and eras differ on exception design (avoid/leverage checked exceptions), null handling (Optional usage), DI approaches (Spring/JSR-330); AI easily picks wrong "standard"

**Recommended Mitigations:**
- Specify framework used (e.g., Spring Boot 3 / Jakarta EE 10) and build tool (Maven/Gradle, DSL type)
- Fix coding conventions (exception policy, Optional/null policy, DI/annotation policy, package structure) briefly in AGENT.md
- Provide reference paths to "representative examples (files containing typical patterns)" in existing code and guide AI to align with them

---

### ② Documentation Consistency

**AI Mis-inference Points:**
- Java SE (Oracle/OpenJDK) official docs, JEPs, framework docs, and old blog posts mix; APIs/recommendations conflict across generations
- Confuses Java EE → Jakarta EE package migration (javax.* → jakarta.*), getting pulled toward old descriptions
- Spring/Security/Hibernate change configuration/recommendations across major versions; AI misidentifies target version in documentation

**Recommended Mitigations:**
- Declare target Java version (e.g., 17/21) and major dependency versions as "fixed values" upfront
- Specify reference priority (e.g., repository docs > official reference > Javadoc > blogs)
- Clarify Jakarta migration status and permitted namespaces (javax/jakarta)

---

### ③ Practice Consistency

**AI Mis-inference Points:**
- Package structure varies greatly (layered: controller/service/repo vs. domain-driven vs. feature-based); AI easily generates code breaking existing structure
- Presence/absence of code generation libraries (Lombok/MapStruct/AutoValue/Immutables) changes "how much code to write"; AI misses assumptions
- Test practices (JUnit 4/5, Mockito, Spring Test, Testcontainers) and naming conventions differ; AI introduces mixing or deprecated patterns

**Recommended Mitigations:**
- Fix adopted architecture (package policy, layers, dependency direction) in one sentence and explicitly prohibit deviations
- Clarify auxiliary tools used (Lombok, etc.) and "allowed/prohibited" usage
- Fix test infrastructure (JUnit generation, mock policy, integration test policy) and instruct alignment with existing test locations/naming

---

### ④ Dependency Stability

**AI Mis-inference Points:**
- Spring Boot / Hibernate / Jackson / Netty require version alignment; AI mixing latest syntax individually easily causes dependency conflicts
- Easily overlooks Java version constraints (e.g., library requires 17+) or breaking changes from Jakarta migration
- Tends to suggest "convenient new dependency additions," breaking alignment with existing BOM/parent POM/version catalogs

**Recommended Mitigations:**
- Document dependency management approach (Maven BOM/parent POM, Gradle version catalog) and state "additions require consultation"
- Specify dependency update rules (Renovate/Dependabot, acceptable ranges, compatibility verification procedures)
- Establish guideline: "first check if achievable with existing dependencies"

---

### ⑤ API Consistency

**AI Mis-inference Points:**
- Many same-name/similar APIs (date-time, collections, HTTP, JSON, logging) exist; AI easily mixes non-standard alternatives
- Mishandling exception models (checked/unchecked, custom exception hierarchy, HTTP error conversion) or return value policies (Optional/nullable) breaks caller code
- Tends to suggest bypassing framework abstractions (Repository/Template/Client) and directly calling low-level APIs

**Recommended Mitigations:**
- Fix "standard API list" (JSON: Jackson, HTTP: WebClient/RestTemplate which one, date-time: java.time, logging: SLF4J, etc.)
- Document error handling conventions (exception throw/catch, error codes, validation) in AGENT.md
- Establish rule to prioritize existing abstraction layers (don't introduce new clients)

---

### ⑥ Ecosystem Consistency

**AI Mis-inference Points:**
- Diverse execution platforms (server, container, Lambda, Android, OSGi, module presence/absence); assumptions misalignment causes configuration/I/O incompatibilities
- Environments with constraints like Java modules (JPMS) or native images (GraalVM) restrict reflection/dynamic generation; AI's generic solutions fail
- Logging (SLF4J + Logback/Log4j2), configuration (Spring Config/MicroProfile Config), DI—"combinations" change correct answers

**Recommended Mitigations:**
- Specify deployment form (JVM only/native/container), runtime environment constraints (K8s, FIPS, proxy, memory limits)
- Fix infrastructure set used (DI, config, HTTP, ORM, logging) and prohibit mixing alternatives arbitrarily
- Declare reflection/dynamic proxy availability (GraalVM, etc.); document prohibitions/restrictions if present

---

### ⑦ Static Semantic Service

**AI Mis-inference Points:**
- Java is statically typed but generics type erasure, raw array/generic mixing, wildcards make inference fragile
- Annotation-driven (DI/ORM/serialization) and code generation (Lombok) have much "invisible semantics"; AI assumes non-existent methods/fields
- Null safety poorly expressed in types; Optional usage variations lead to incorrect handling

**Recommended Mitigations:**
- Document "static analysis enforcement assumptions" (Error Prone / SpotBugs / Checkstyle / Nullness annotations adoption)
- When using Lombok/generation tools, explicitly state generated elements (getters/setters/constructors) as repository conventions
- Fix null/Optional policy (where to use in fields/parameters/returns)

---

### ⑧ Runtime Semantic Service

**AI Mis-inference Points:**
- Reflection, classloaders, proxies (Spring/Hibernate) create runtime-only behaviors (lazy loading, transaction boundaries, circular references)
- Checked/unchecked exception mixing without wrapping/rethrowing/logging conventions easily causes operational issues
- Concurrency (threads, pools, CompletableFuture, virtual threads) with execution order and context propagation causes AI to oversimplify

**Recommended Mitigations:**
- Document transaction boundaries, lazy loading policy, session management (Open Session in View adoption)
- Fix exception and logging operational conventions (no suppression, wrapping layers, monitoring integration)
- Specify concurrency standards (Executor management, virtual thread adoption, timeout policy)

---

### ⑨ Core Semantic Consistency

**AI Mis-inference Points:**
- Java language features vary greatly across generations (8/11/17/21…); AI suggests assuming records/sealed/pattern matching/var/virtual threads incompatible with target
- Compatibility maintenance history leaves old APIs/practices; choice errors on what to use
- Even "same Java" on JVM, vendor differences, flags, GC, module boundaries can change behavior/constraints

**Recommended Mitigations:**
- Document target Java version and available language features (adopted/prohibited)
- Fix "standard API generation to use" (e.g., use java.time, don't touch old Date/Calendar)
- Declare runtime JDK (Temurin/Oracle, etc.) and major runtime settings (GC, --add-opens necessity, module usage)

---

## Summary

This evaluation demonstrates how AI struggles with Java's ecosystem diversity when given minimal context. The ecosystem has evolved significantly over decades, creating multiple valid approaches (frameworks, build tools, dependency management) that AI can easily confuse without explicit guidance.

**Key Takeaway:**  
Java's long history and diverse ecosystem (Spring vs Jakarta EE, Maven vs Gradle, javax vs jakarta) require explicit framework and version declarations. Without clear project conventions, AI suggestions can mix incompatible patterns across different Java generations and ecosystems.
