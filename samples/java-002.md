# Java Ecosystem Evaluation Sample

**AI Model:** GPT-5.2  
**Date:** 2024-01-15

---
## Information to Provide as Input
- Language name
    - Java
- Language version
    - Java 8
- Major libraries used
    - Spring Boot
    - Spring MVC
    - Hibernate (JPA)
    - Lombok
    - Maven
- Runtime environment
    - **Server:** Embedded Tomcat (Spring Boot)
    - **OS:** Linux
    - **Database:** MySQL
- Project architecture overview
    - **Controller Layer:** REST API
    - **Service Layer:** Business logic
    - **Repository Layer:** DB access via JPA/Hibernate
    - **Entity Layer:** DB mapping
    - **Structure:** Three-tier architecture based configuration

---

## Evaluation Results

### ① Community Consistency

**AI Mis-inference Points:**
- Spring Boot / Spring MVC "recommended layering" and exception handling approaches (ControllerAdvice, etc.) vary by team and era; AI tends to select different schools (e.g., putting business logic in Controller, returning Entity directly)
- JPA/Hibernate operational practices (Fetch strategies, DTO projections, transaction boundaries, N+1 countermeasures) vary greatly across projects; AI mixes in common but project-incompatible approaches
- Lombok tolerance (usage scope, @Data acceptability, Builder usage) differs per team; AI aggressively pushes Lombok and disrupts design

**Recommended Mitigations:**
- Document "this project's style" explicitly (e.g., use/don't use Entity in API responses, presence of DTO conversion layer, @Transactional only in Service layer)
- Fix layer responsibilities (Controller/Service/Repository/Entity) and exception/validation placement as rules in AGENT.md
- List permitted Lombok annotations and prohibitions (e.g., avoid @Data)

---

### ② Documentation Consistency

**AI Mis-inference Points:**
- Spring version differences are large; AI easily mixes knowledge from Spring Boot 3.x docs (jakarta.* based) incompatible with Java 8
- Hibernate/JPA "Hibernate-specific features" vs. "JPA standard" easily blur; AI treats non-standard APIs as standard
- MySQL dialect/timezone/charset recommendations vary across articles; AI suggests configurations inconsistent with environment assumptions

**Recommended Mitigations:**
- Always have AI reference actual Spring Boot / Spring / Hibernate versions from pom.xml, with rule to prioritize "official reference for that version only"
- Specify namespace policy like "use javax.*, not jakarta.*" (prevents incorrect references for Java 8 + legacy generation)
- Fix DB-related policies (timezone, collation, Dialect, DDL approach, naming conventions) and don't let AI guess

---

### ③ Practice Consistency

**AI Mis-inference Points:**
- REST design (status codes, error response format, pagination, PATCH/PUT usage) easily fluctuates; AI changes style per endpoint
- Exception handling and logging (suppressing exceptions, stack trace handling, log granularity) variations cause AI to add try-catch ad hoc
- Repository/Service responsibility division (complex logic in Repository vs. Service) fluctuations lead AI to generate cross-layer implementations

**Recommended Mitigations:**
- Fix API specifications (error JSON format, common response presence, status code table) and define common endpoint template in text
- Document exception "catch layers," "transformation layers (ControllerAdvice)," and "logging policy"
- Present naming conventions, package structure, and DTO/Entity/VO roles as conventions

---

### ④ Dependency Stability

**AI Mis-inference Points:**
- Spring Boot major differences (2.x vs 3.x) require different Java versions; AI dependency update suggestions can immediately break builds (Java 8 assumption collapse)
- Hibernate behavior (generated SQL, dialects, lazy loading, ID generation, NamingStrategy) changes per version; AI suggests generic configurations causing issues
- Lombok has IDE/compiler/annotation processing compatibility; AI assumes "just add dependency" works

**Recommended Mitigations:**
- Write compatibility constraints at top level: "Java 8 fixed," "Spring Boot 2.x series fixed (specify actual version)"
- Establish dependency update procedure rules: "check dependency:tree and compatibility matrix first," "follow Boot BOM"
- Document Lombok prerequisites: "annotation processing required," "pass javac build in CI"

---

### ⑤ API Consistency

**AI Mis-inference Points:**
- Spring MVC annotations and argument resolution (@RequestBody/@RequestParam/@PathVariable, BindingResult) have many similar patterns; AI produces subtly inconsistent combinations
- JPA has many choices (EntityManager/Repository/QueryDSL); AI mixes in project-unadopted APIs
- Transaction (@Transactional) propagation, readOnly, exception rollback conditions—subtle API contracts AI easily misunderstands

**Recommended Mitigations:**
- Explicitly state adopted APIs (e.g., use/don't use Spring Data JPA, don't use Criteria). List unadopted options as prohibited
- Fix @Transactional application policy (which layers, readOnly criteria, checked exception handling) in text
- Unify Controller input/output type policy (DTO only, etc.) and Validation application method as standard rule

---

### ⑥ Ecosystem Consistency

**AI Mis-inference Points:**
- Maven (plugins, profiles, resource filtering) × Spring Boot (repackage) × Lombok (annotation processing) combination causes AI to mix "Gradle assumptions" or "IDE-dependent" solutions
- Embedded Tomcat and Linux operation assumptions (log output, ports, environment variables, JVM options) easily get misreplaced with container/cloud assumptions by AI
- MySQL connection (driver, SSL, charset, serverTimezone) configuration is environment-dependent; AI suggests generic JDBC URL that misaligns

**Recommended Mitigations:**
- Document correct build/startup (mvn commands, Jar launch method, required environment variables, application.properties priority)
- Fix Maven plugins and versions used, Java 8 source/target settings, Lombok handling
- Provide DB connection string and required parameters (timezone, etc.) as template

---

### ⑦ Static Semantic Service

**AI Mis-inference Points:**
- Java has strong type information, but Lombok-generated methods/constructors aren't visible in source; AI proceeds assuming "non-existent methods"
- Hibernate entities can be proxied, so while statically they look like simple POJOs, equals/hashCode/toString design is critical—AI overlooks this
- Under Java 8 constraints, AI suggests designs assuming later language features (var, records, sealed, switch expressions)

**Recommended Mitigations:**
- Specify Java 8 available syntax/standard API scope (provide prohibited feature list)
- Provide Lombok adopted annotation list and "what gets generated" as prior knowledge (don't let AI guess)
- Specify Entity equals/hashCode/toString policy (ID-based, etc.) and rule not to touch Lazy associations in text

---

### ⑧ Runtime Semantic Service

**AI Mis-inference Points:**
- Spring's AOP/proxies mean @Transactional "doesn't work on same-class internal calls"—AI ignores such runtime traps
- Hibernate Lazy Loading only breaks at runtime (LazyInitializationException); AI creates dangerous paths like "return Entity from Controller"
- MySQL timezone/collation differences cause runtime-only issues; AI underestimates environment differences

**Recommended Mitigations:**
- Specify transaction boundaries and session lifetime (Open Session in View enabled/disabled) and only allow suggestions under that assumption
- Fix Lazy/Fetch operation rules (don't return Entity directly in API responses, finalize required data in Service layer)
- Present runtime environment assumptions (Linux, MySQL config, JDBC URL required items) as "unchangeable premises"

---

### ⑨ Core Semantic Consistency

**AI Mis-inference Points:**
- Java 8 is old but stable; AI easily mixes newer Java conventions (modules, latest GC-based optimizations, jakarta-ized peripheral knowledge)
- null/Optional, Stream side effects, date-time API (java.time) show project-specific styles; AI generates inconsistent designs (using Optional in fields, etc.)
- Exception design (checked/unchecked) and serialization (Jackson and Java type interaction) lead AI to create inconsistencies with generic assumptions

**Recommended Mitigations:**
- Document era boundary like "complete within Java 8 and its standard library scope," "maintain javax namespace"
- Define and present Optional/Stream/date-time type usage conventions (where to use, types in DTOs, null handling)
- Fix JSON conversion policy (date format, null output, naming strategy) and exception→error response mapping conventions and provide to AI

---

## Summary

This evaluation demonstrates how the 9-axis model identifies structural vulnerabilities in a Java 8 + Spring Boot + Hibernate + Maven environment. The legacy Java version constraint (Java 8) combined with modern framework expectations creates particularly complex challenges for AI coding assistants.

**Key Takeaway:**  
When working with legacy Java versions, explicitly documenting version constraints, framework-specific patterns, and ecosystem tool configurations is critical. AI easily mixes knowledge from incompatible versions (Spring Boot 3.x, jakarta namespace) if not properly constrained.
