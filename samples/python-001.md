# Python Ecosystem Evaluation Sample (Minimal Context)

**AI Model:** GPT-5.2  
**Date:** 2024-01-15

---
## Information to Provide as Input
- Language name
    - Python
---

## Evaluation Results

### ① Community Consistency

**AI Mis-inference Points:**
- Virtual environment practices (venv / virtualenv / conda) and package management (pip / Poetry / uv / pip-tools) vary widely; AI tends to suggest procedures/configurations that don't match project assumptions
- Common patterns for testing (unittest / pytest), logging (logging usage), and configuration (dotenv / pydantic-settings) have multiple variations, leading to context mismatches
- AI often imports general "Python best practices" (scripts in root, global execution, excessive relative imports)

**Recommended Mitigations:**
- Explicitly state "adopted toolchain" (e.g., venv + pip, Poetry, uv, or conda / execution commands)
- Fix standards for testing, linting, formatting, and type checking (pytest / ruff / black / mypy or pyright)
- Document project directory conventions (src layout presence/absence, import policy, entry points) in AGENT.md

---

### ② Documentation Consistency

**AI Mis-inference Points:**
- Python version differences (3.8–3.12) change standard library and typing specifications/recommendations, but AI often references docs from different versions
- Official docs, PEPs, blog articles, and library READMEs sometimes conflict; AI may import outdated practices (setup.py assumptions, etc.)
- "Same-named features" exist across contexts (configuration, CLI, HTTP clients), leading to doc mixing

**Recommended Mitigations:**
- Specify target Python version range (e.g., Python 3.11 fixed / 3.10–3.12 supported) and verify in CI
- Create a list of authoritative references (official docs for the target version, adopted library official docs)
- Also specify "not adopted" mechanisms (e.g., no direct setup.py editing, certain config approaches not used)

---

### ③ Practice Consistency

**AI Mis-inference Points:**
- Module structure (single file / package / src/ layout) varies easily, leading to suggestions that cause import errors or circular dependencies
- Exception design, return value design (None / exceptions / Result-like), and data structures (dataclass / pydantic / dict) choices often misalign with project style
- "Pythonic" as a vague standard can lead to brevity-over-readability suggestions

**Recommended Mitigations:**
- Document package boundaries, dependency directions, and import conventions (absolute imports, relative import policy)
- Specify exception policy (where to suppress, custom exception naming) and return value policy
- Define adopted data models (dataclass / attrs / pydantic) and their use cases (I/O, internal representation)

---

### ④ Dependency Stability

**AI Mis-inference Points:**
- Python dependencies update rapidly; same library can have breaking changes in major updates, yet AI tends to assume latest API
- Underestimates OS/architecture dependencies (C extensions, build requirements), suggesting dependencies that work locally but break in CI/production
- Assumes "no lock file" operation, accumulating "pip install..." commands and breaking reproducibility

**Recommended Mitigations:**
- Specify dependency pinning strategy (lockfile mandatory, upper bounds, constraints)
- Establish rules for "new dependency addition" (approval criteria, review requirements, prefer stdlib alternatives)
- Document target OS/runtime environments (Linux/Windows/macOS, Docker presence) and build prerequisites

---

### ⑤ API Consistency

**AI Mis-inference Points:**
- Same-concept APIs proliferate (sync/async, bytes/str, pathlib/os.path, datetime/pendulum), leading to mixed calls that don't match project-adopted APIs
- Mishandles exception types, return values, mutable/immutable treatment, leading to "silent bug" suggestions (destructive changes, default argument traps)
- Type annotation adoption level (strict/loose) affects expected function signatures and Nullable handling

**Recommended Mitigations:**
- List standard adopted APIs (e.g., use pathlib, datetime requires timezone awareness)
- Clarify "boundary layer" conventions (how to handle types, exceptions, encoding in I/O, external APIs, DB layers)
- Share type checking policy (mypy/pyright configuration, strictness level, Optional handling)

---

### ⑥ Ecosystem Consistency

**AI Mis-inference Points:**
- Build/distribution (setuptools / Poetry / Hatch / Flit), execution (python -m, entry points), and configuration (pyproject.toml vs. separate files) toolchain differences are large; AI suggests procedures from different schools
- Design changes depending on Web/non-Web, WSGI/ASGI, asyncio presence, but AI misunderstands assumptions when suggesting architecture
- Easily disrupts existing lint/format configurations (ruff/flake8/isort/black combinations)

**Recommended Mitigations:**
- Fix adopted tools (build backend, task execution, CI) and present commands in copy-pasteable form
- Specify async policy (use/don't use, boundaries if used)
- Treat existing config files (pyproject.toml, etc.) as "single source of truth" and establish rules for additions/changes

---

### ⑦ Static Semantic Service

**AI Mis-inference Points:**
- Python type information is optional; implementation and type annotations can diverge, leading AI to make incorrect assumptions based on types alone
- Library type stub deficiencies/inconsistencies cause AI to assume "types exist" when designing
- Dynamic mechanisms (__getattr__, decorators, metaclasses) make static analysis difficult; AI misreads these areas

**Recommended Mitigations:**
- Specify type checking tool and strictness level, required scope for type annotations (public APIs only, etc.)
- Define boundary between "what types guarantee / what tests guarantee"
- Document areas using mechanisms that elude static analysis (dynamic imports, plugin systems)

---

### ⑧ Runtime Semantic Service

**AI Mis-inference Points:**
- Dynamic typing and duck typing delay failures until runtime; AI tends to assume "should work"
- Diverse exceptions with unclear handling, rethrowing, and logging granularity policies lead to operationally dangerous handling suggestions
- Easily overlooks runtime environment dependencies (timezones, locales, file encoding, concurrency models: threads/processes/async)

**Recommended Mitigations:**
- Standardize exception handling policy (which exceptions to catch, logging, retries, user-facing errors)
- Document runtime environment assumptions (OS, TZ, encoding, container presence) and reproduction procedures
- Require critical logic to have test types (unit/integration) and observability (logs/metrics) as a package

---

### ⑨ Core Semantic Consistency

**AI Mis-inference Points:**
- Python values backward compatibility while gradually changing syntax, standard library, and typing per version; AI mixes conventions from different versions
- Ambiguities that "just work" (implicit type conversion is rare, but dynamic attribute addition, mutable default arguments exist) lead to subtle bug-containing suggestions
- Easily assumes implementation-specific behavior (CPython-specific behavior, performance characteristics)

**Recommended Mitigations:**
- Specify target Python implementation (usually CPython) and supported versions, fixed in CI
- Checklist forbidden patterns (mutable default arguments, ambiguous datetime, implicit str/bytes mixing)
- Document performance/concurrency assumptions (GIL-aware design, I/O async policy) as project policy

---

## Summary

This evaluation demonstrates how AI coding assistants can struggle with Python's ecosystem diversity when given minimal context (language name only). The 9-axis analysis reveals specific structural vulnerabilities and provides concrete mitigation strategies.

**Key Takeaway:**  
Python's flexibility and diverse ecosystem require explicit project conventions to guide AI effectively. Without clear documentation of toolchain choices, API standards, and runtime policies, AI suggestions can introduce subtle incompatibilities.
