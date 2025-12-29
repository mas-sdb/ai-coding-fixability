# Model Usage Guide - Choosing Evaluation Perspectives

## Table of Contents

- [Purpose of This Guide](#purpose-of-this-guide)
- [Basic Policy](#basic-policy-use-independently-combine-optionally)
- [1. 4 Layers (Semantic Layers)](#1-4-layers-semantic-layers---structural-understanding-of-language-ecosystems)
- [2. 9 Axes (Fixability Axes)](#2-9-axes-fixability-axes---evaluating-ai-fixability)
- [3. 7 Phases (AI Verification Loop)](#3-7-phases-ai-verification-loop---ai-verification-process)
- [4. How to Combine](#4-how-to-combine-optional)
- [5. Customization Principles](#5-customization-principles)
- [6. Converting to Project-Specific Evaluation Axes](#6-converting-to-project-specific-evaluation-axes)
- [7. Conversion Process](#7-conversion-process---7-steps)
- [8. Conversion Process Summary](#8-conversion-process-summary)

---

## Purpose of This Guide

This document is for advanced users who want to understand the relationship between 
Semantic 9 Axes (the model's abstract axes) and the practical 9 axes used in prompts.

Regular users can simply use the provided prompts as-is without needing to understand 
this conversion process.

The reason axis names don't match the model is that Semantic Axes are abstract models, 
while practical axes are reconfigured versions optimized for project diagnosis.

This model consists of two documents:

1. [Evaluation Model](./model.md) - Detailed model definition
2. **This Guide** - How to use the model

**Related Documents:**
- [Design Philosophy](./philosophy.md) - Principles for long-term framework stability
- [Thought Process](./model-thought-process.md) - Development journey and reasoning behind this framework

**Purpose of This Guide:**
- Explain the independence of the model's three perspectives (4 Layers, 9 Axes, 7 Phases)
- Show recommended usage for different users
- Present customization principles

---

## Basic Policy: Use Independently, Combine Optionally

This model has the flexibility to be used by extracting only the parts you need for your purpose.

**Recommendations by User Type:**
- Developers → Using **9 Axes (Fixability Axes) only** is sufficient
- Researchers → Can discuss using **4 Layers (Semantic Layers) only**
- AI Agent Designers → Can optimize using **7 Phases (AI Verification Loop) only**
- Advanced Users → Can analyze by combining **4 Layers × 9 Axes × 7 Phases**

Each perspective stands independently, and you don't need to understand the entire framework.

---

## 1. 4 Layers (Semantic Layers) - Structural Understanding of Language Ecosystems

**Detailed Definition:** 4. Four-Layer Structure (Semantic Layers)

The 4 Layers are a theoretical and research-oriented perspective for understanding language ecosystems structurally.

**The Four Layers:**
- **Core Layer** - Language specification, type system, basic syntax
- **Service Layer** - Compilers, toolchains, development support tools
- **Dependency Layer** - Dependency management, package ecosystems
- **Community Layer** - Shared knowledge, best practices, community culture

### Applications of 4 Layers

**Best Used For:**
- Cross-language comparative analysis
- Understanding language design philosophy
- Analyzing historical background of ecosystems
- Research, papers, education

**Note:**
Not essential for field diagnostics. Use when you want to understand theoretical background.

---

## 2. 9 Axes (Fixability Axes) - Evaluating AI Fixability

**Detailed Definition:** 9 Axes × 2 Dimensions Model (Implementation / Runtime)

The 9 Axes are practical tools for diagnosing AI fixability on a per-project basis.

**The Nine Evaluation Axes:**

1. **Public Knowledge** - Quality and quantity as training data, affects initial generation
2. **Static Semantics** - Quality of implementation-time semantic verification
3. **Spec Conformance** - Consistency of language specifications
4. **Tool Support** - Richness of development tools
5. **Error Clarity** - Quality of error messages
6. **Runtime Continuity** - Consistency between implementation time and runtime
7. **Dependency Management** - Quality of package management
8. **Environment Semantics** - Clarity of environment settings
9. **Runtime Semantics** - Clarity of runtime behavior

### Applications of 9 Axes

**Best Used For:**
- Per-project language diagnostics
- Designing AGENT.md / instructions.md
- Re-evaluation when dependencies update
- Practical Fixability improvement

**Recommendation:**
Using only the 9 Axes is sufficient in the field. You can perform diagnostics without understanding the theoretical background of the 4 Layers.

---

## 3. 7 Phases (AI Verification Loop) - AI Verification Process

**Detailed Definition:** [model.md - Section 4.1](./model.md#41-definition-of-7-phases)

The 7 Phases decompose the process by which AI generates, verifies, and fixes code.

**Seven Stages of the Verification Loop:**

```
① Static Knowledge (Prior Knowledge)
   - AI training data, language specs, common patterns

② Generation (Initial Generation)
   - Initial code generation by AI

③ Static Semantic Verification
   - Type checking, syntax validation, linter
   - Failure → Go to ⑦

④ Startup Check
   - Build, dependency resolution, startup confirmation
   - Failure → Go to ⑦

⑤ Test Execution
   - Unit tests, integration tests

⑥ Runtime Feedback
   - Test results, error messages, traces
   - Success → End / Failure → Go to ⑦

⑦ Regeneration (Corrective Generation)
   - Fix based on feedback
   - → Return to ③ and loop
```

### Applications of 7 Phases

**Best Used For:**
- AI agent design
- Improving correction loops
- Prompt design
- Optimizing runtime feedback

**Note:**
Not essential for language diagnostics itself. Use when you want to understand or improve AI agent behavior.

---

## 4. How to Combine (Optional)

You can combine them as needed in the following ways.

### A. 9 Axes × 7 Phases (Field-Oriented)

**Purpose:**
Map which axes become obstacles at which phases during correction.

**Output:**
Project-specific fixability map

**Use Cases:**
- Public Knowledge (Axis 1) is insufficient in Static Knowledge Phase (①)
- Error Clarity (Axis 5) is unclear in Static Semantic Verification Phase (③)
- Runtime Continuity (Axis 6) is inconsistent in Runtime Feedback (⑥)

### B. 4 Layers × 9 Axes (Research-Oriented)

**Purpose:**
Analyze which layer fluctuations appear in which axes.

**Output:**
Structural understanding of language ecosystems

**Use Cases:**
- Community Layer fluctuations → Decline in Public Knowledge (Axis 1)
- Service Layer deficiencies → Decline in Error Clarity (Axis 5)
- Dependency Layer instability → Decline in Dependency Management (Axis 7)

### C. 4 Layers × 7 Phases (AI Agent Design-Oriented)

**Purpose:**
Organize which layer information is needed at which phase.

**Output:**
Information acquisition strategy for AI agents

**Use Cases:**
- Static Knowledge Phase (①) → Community Layer
- Static Semantic Verification Phase (③) → Core + Service Layer
- Startup Check Phase (④) → Dependency Layer

### D. 4 Layers × 9 Axes × 7 Phases (Advanced Analysis)

**Purpose:**
Comprehensively analyze language, ecosystem, and AI reasoning processes.

**Output:**
Comprehensive fixability analysis

**Use Cases:**
Used when language designers, researchers, and AI tool developers collaborate to improve language ecosystems.

---

## 5. Customization Principles

This framework can be arranged in any way.
However, maintaining the following four principles will preserve the framework's consistency.

### Principle 1: Center on Fixability

The evaluation focus is always "Can AI fix mistakes?"
Evaluate the convergence of the correction loop, not initial generation quality.

### Principle 2: Evaluate Transparency of Semantic Information

Evaluate "What gets conveyed to AI?" rather than "What to evaluate in the language?"
Clarity of error messages, type information, and documentation is important.

### Principle 3: Don't Make Language Values into Evaluation Axes

Don't bring value conflicts like static vs. dynamic typing or object-oriented vs. functional into evaluation.
All values are treated equally as "methods of providing semantics."

### Principle 4: No Scoring or Ranking

This framework is not for determining language superiority.
It's a tool for understanding each language's characteristics and making appropriate project choices.

---

## Summary (Basic Edition)

This framework is designed so that you can use just one of the 4 Layers, 9 Axes, or 7 Phases.

**Recommendations by User Type:**
- **Field Developers** → Use only the 9 Axes
- **Researchers** → Use only the 4 Layers
- **AI Agent Designers** → Use only the 7 Phases
- **Advanced Analysis** → Combine freely

This flexibility is the greatest strength of a language ecosystem evaluation model for the AI era.

---

## Practical Tools

Tools for actually using this model:

### diagnostic_prompt.md
- Language diagnostic prompt based on 9 axes (adjusted for real project use)
- Usable with any AI model
- Usage details: [diagnostic_prompt.md](./diagnostic_prompt.md)

### Utilizing Diagnostic Results
Based on diagnostic results, you can design AGENT.md and instructions.md to improve project-specific fixability.

---

## 6. Converting to Project-Specific Evaluation Axes

This section explains how to convert theoretical **Semantic 9 Axes** into **practical 9 axes** 
usable for actual project diagnostics.

### Why Conversion is Necessary

Semantic 9 Axes (the evaluation axes of this model) are abstract semantic models for analyzing AI reasoning structures.
However, in practice, the following are needed:

- **Axes that can classify project-specific misreasoning**
- **Axes that teams can understand easily**
- **Axes that can be implemented in AGENT.md or instructions.md**

Therefore, a conversion process is needed to reconfigure Semantic 9 Axes for practical use.

**Note:**
You may use the model's axes as-is, but highly abstract answers may result.
By redefining axes to match project-specific contexts, more concrete and practical diagnostics become possible.

---

## 7. Conversion Process - 7 Steps

### Step 1: Convert Abstract Concepts to "Causes of Misreasoning"

Since Semantic Axes are abstract, first convert them to **"causes of AI misreasoning."**

**Examples:**
- Public Knowledge → Fluctuations in information freshness and quality
- Static Semantic Consistency → Type and structure consistency
- Runtime Semantic Continuity → Runtime state, async, runtime differences

**Purpose: Redefine abstract concepts as "sources of misreasoning"**

---

### Step 2: Observe How Misreasoning Appears in the Field

Next, make concrete **how abstract misreasoning causes appear in actual projects.**

**Examples:**
- AI mixes different patterns due to community school differences
- AI uses old APIs due to documentation version differences
- AI generates dangerous code by misunderstanding runtime behavior

**Purpose: Bridge from abstract to concrete**

---

### Step 3: Reorganize Concrete Misreasoning into "Field-Friendly Categories"

**This is the most important step.**

Reorganize observed misreasoning into **categories that are easy to understand and diagnose in the field.**

**Examples:**
- Community school differences → Community Consistency
- Documentation version differences → Documentation Consistency
- Runtime behavior differences → Runtime Semantic Service

**Purpose: Convert abstract model to practical model**

---

### Step 4: Axis Names Can Be Changed Freely (Essence is Classification Content)

Axis names are just labels, so **feel free to change them to names that are understandable for your project.**

As long as the meaning of Semantic Axes is preserved, you can freely change axis names.

**Examples:**
- Semantic Metadata Richness → Documentation Consistency
- Semantic Extensibility → API Consistency

**Purpose: Make axes field-friendly**

---

### Step 5: Create Mapping Between Semantic Axes and Practical Axes

Finally, create a correspondence table between Semantic Axes and practical axes.

**Example Mapping Table:**

| Semantic 9 Axes (Abstract) | Practical 9 Axes (Example) | Meaning of Conversion |
|----------------------------|----------------------------|------------------------|
| **1. Public Knowledge** | **Community Consistency** | Community knowledge fluctuations, school differences, outdated information mixing |
| **2. Static Semantics** | **Static Semantic Service** | Static consistency of types, structures, dependencies |
| **3. Metadata Richness** | **Documentation Consistency** | Consistency of annotations, comments, official documentation |
| **4. Access & Automation** | **Ecosystem Consistency** | Implicit behaviors of DI, build, pipeline, toolchain |
| **5. Runtime Continuity** | **Runtime Semantic Service** | Runtime state, async, runtime differences |
| **6. Dependency Stability** | **Dependency Stability** | Update frequency and compatibility of dependencies (as-is) |
| **7. Spec Conformance** | **Ecosystem Consistency (part)** | OS, cloud, hosting specification differences |
| **8. Compatibility Culture** | **Practice Consistency** | Legacy culture, old patterns, field practice differences |
| **9. Semantic Extensibility** | **API Consistency** | Semantic extensions like DSL, extension methods, events, pipelines |

**Note:**
This is just an example. Feel free to change axis names and classifications according to project characteristics.

---

### Step 6: Fine-Tune to Match Your Project

Fine-tune practical axes to match project-specific elements (language, framework, architecture).

**Examples:**

**For C#/.NET Projects:**
- Implicit behaviors of DI / Middleware / Entity Framework Core / Azure
- ASP.NET Core configuration conventions
- NuGet package update patterns

**For Java/Spring Projects:**
- Implicit behaviors of AOP / Bean / Hibernate / Tomcat
- Spring Boot auto-configuration
- Maven/Gradle dependency resolution

**For Python Projects:**
- Partial application of type hints
- pip/conda dependencies
- Runtime dynamic typing

**Purpose: Create axes that can catch project-specific misreasoning**

---

### Step 7: Create Project Diagnostic Report

Finally, create a **Project Diagnostic Report** using the converted practical 9 axes.

This report documents points where AI is likely to misreason and mitigation strategies.

---

#### 7-1. Report Purpose

The Project Diagnostic Report is created for the following purposes:

1. **Visualize AI Misreasoning Points**
   - Identify project-specific misreasoning risks
   - Share across the team

2. **Document Mitigation Strategies**
   - Source material for AGENT.md and instructions.md
   - Reflection in coding standards

3. **Record Diagnostic Results**
   - Baseline for re-evaluation when dependencies update
   - Track changes over time

#### 7-2. Key Points for Report Creation

**Important Policies:**

1. **Avoid Quantitative Evaluation**
   - Don't use ⭐ ratings or scores
   - Use qualitative expressions like "excellent" or "room for improvement"

2. **Based on Structural Observation**
   - Structural facts, not value judgments
   - Impact on AI reasoning processes

3. **Practical Mitigation Strategies**
   - Concrete and executable actions
   - Specify implementation location (AGENT.md, instructions.md, etc.)

4. **Continuous Updates**
   - Re-evaluate when dependencies update
   - Add new misreasoning patterns

---

#### 7-3. How to Utilize the Report

Utilize the created diagnostic report as follows:

**Reflection in AGENT.md:**
````markdown
# Project-Specific Guidelines

## Patterns to Use
- ASP.NET Core Web API (Do not use Minimal API)
- Entity Framework Core (Use Database First)

## Notes
- Nullable reference types are enabled (.csproj has <Nullable>enable</Nullable>)
- Actively use C# 14 features
````

**Reflection in Coding Standards:**
````markdown
# ASP.NET Core Implementation Standards

## API Design
- ✅ Use controller-based
- ❌ Do not use Minimal API

## Entity Framework
- ✅ Use Database First
- ❌ Do not use Code First
````

**Checklist for Dependency Updates:**
````markdown
- [ ] Review diagnostic report
- [ ] Verify no new misreasoning patterns
- [ ] Check if AGENT.md needs updating
````

---

## 8. Conversion Process Summary

```
1. Understand abstract concepts of Semantic Axes
2. Make concrete the misreasoning they produce
3. Reorganize misreasoning into field-friendly categories
4. Axis names can be changed freely (essence is classification content)
5. Create mapping between Semantic Axes and practical axes
6. Fine-tune with project-specific elements
7. Complete as practical 9-axis diagnostic template
```

**Important Principles:**
- Semantic 9 Axes are the "theoretical foundation"
- Practical 9 axes are "project-specific application"
- They don't need to be in one-to-one correspondence
- Freely customizable to match project context

---

## Summary (Conversion Process Edition)

This model can be used at three levels:

**Level 1: Use As-Is**
- Use Semantic 9 Axes directly for diagnostics
- Abstract but highly versatile

**Level 2: Light Customization**
- Change axis names to match project
- Make evaluation criteria concrete

**Level 3: Full Customization**
- Redefine practical 9 axes following Steps 1-7
- Create project-specific diagnostic template

Choose which level to use based on project scale, team proficiency, and diagnostic purpose.
