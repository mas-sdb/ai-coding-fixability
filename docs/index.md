--- 
title: "English" 
---
### *Fixability × Semantic Transparency = Stable AI-Assisted Coding* 

[🌐 English](./index.md) | [🌐 日本語](./ja/index.md)

AI-assisted coding is not defined by how accurately AI writes code on the first attempt.  
Its true productivity comes from **how reliably AI can correct its own mistakes (Fixability)**.  
And what determines this correction capability is  
**the quantity and clarity of information AI needs to correctly understand the context (Semantic Transparency)**.

This framework is designed to systematically explain the stability of AI code generation  
through the two axes of **"Fixability × Semantic Transparency"**.

---

## 📚 **Table of Contents**

- [Why This Framework Matters](#-why-this-framework-matters)
- [Framework Overview](#-framework-overview)
- [Diagnostic Prompt](#-diagnostic-prompt)
- [Next Steps](#-next-steps)
- [Summary](#-summary)
- [Deep Dive: Background and 9-Axis Diagnostic Framework](#-deep-dive-background-and-9-axis-diagnostic-framework)
- [About This Framework](#-about-this-framework)

---

## 🔍 **Why This Framework Matters**

### The Real Challenge: "AI Behavior Tuning Erodes Productivity"

As a technical consultant and developer using AI coding agents in practice, I've observed the following challenges:

- Time saved by AI writing code < Time spent tuning AI behavior
- Reliance on "personal intuition (gut feeling)" and "prompt magic (luck)"
- Project productivity becomes hit-or-miss and doesn't scale organizationally

This explanatory model emerged from a desire to make this problem structurally explainable.

### A Paradigm Shift in Root Cause

> It's not "AI makes mistakes because it's immature"  
> but rather **"AI lacks sufficient information to make correct judgments"**

With this perspective, instead of relying on personal intuition or magic prompts,  
you can explain **what information was provided to AI that led to this result**  
as a **deficiency in context design**.

### The Value of This Framework

- ✅ Explain stability with reproducible logic
- ✅ Structure the causes of misreasoning, escaping ambiguous "gut feelings"
- ✅ Derive countermeasures systematically, preventing improvement from becoming person-dependent
- ✅ Universal principles resilient to technology and model changes

---

## 🧩 **Framework Overview**

This site introduces an overview of the following components:

### **1. Fixability**
What matters in AI code generation is not "getting code right on the first try"  
but rather **"whether it can be correctly fixed when wrong"**.

The more stable the correction loop converges, the better AI's output quality and work speed.

### **2. Semantic Transparency**
For AI to correct properly, it needs materials to understand the situation:

- Where did it fail? (error content)
- What happened during execution? (logs)
- What is the expected behavior? (test results)
- Technology versions and project configuration
- Input/output examples

The more complete these are, the less AI misreasons, and the more stable the correction loop.

### **3. Fixability × Semantic Transparency Interaction**
These two are not independent but strongly interconnected:

- High Semantic Transparency → AI correctly understands errors → Fixability increases
- Low Semantic Transparency → AI corrects in wrong context → Fixability decreases

In other words, Fixability is not just about AI's capability—**Semantic Transparency is a human-side information design problem**.

### **4. Model-Agnostic Diagnostic Prompt (9-Axis Diagnosis)**
Systematically identify points where AI is likely to misreason for each project.

**Input:**
- Language & version
- Major libraries
- Runtime environment
- Architecture overview

**Output:**
- Points where AI is likely to misreason
- Specific countermeasures for those points

---

## 🧪 **Diagnostic Prompt**

When you input your project's language, version, libraries, and runtime environment,  
a **model-agnostic diagnostic prompt** automatically identifies  
points where AI is likely to misreason and provides countermeasures.

👉 **[Use the Diagnostic Prompt](https://github.com/mas-sdb/ai-coding-fixability/blob/main/diagnostic_prompt.md)**

---

## 🚀 **Next Steps**

### **Understand the Details**
Explore the theoretical background and thought process of the framework.

- [Evaluation Model Details](https://github.com/mas-sdb/ai-coding-fixability/blob/main/model.md) - Complete explanation of 4-layer, 9-axis, verification loop
- [Usage Guide](https://github.com/mas-sdb/ai-coding-fixability/blob/main/model-usage-guide.md) - How to use the framework effectively
- [Thought Process](https://github.com/mas-sdb/ai-coding-fixability/blob/main/model-thought-process.md) - Background of how this model was born
- [Diagnostic Samples](https://github.com/mas-sdb/ai-coding-fixability/tree/main/samples) - Learn from real examples

### **View the Full Repository**
Detailed documentation, diagnostic prompts, and samples  
are published in the GitHub repository.

👉 **[GitHub Repository](https://github.com/mas-sdb/ai-coding-fixability)**

---

## 📋 **Summary**

| Point | Explanation |
|-------|-------------|
| **Challenge** | AI code generation instability stems not from "AI immaturity" but from missing contextual information |
| **Key** | Stability of correction loops (**Fixability**) |
| **Solution** | Enhance semantic transparency (**Semantic Transparency**) |
| **Diagnosis** | Proactively identify project-specific risks with 9-axis diagnostic prompt |

By focusing not on "how to use AI" but on **"information misalignment between AI and humans"**,  
you can stably improve AI coding productivity and quality.

---

## 📚 **Deep Dive: Background and 9-Axis Diagnostic Framework**

### Background: Why This Framework Is Needed

With the emergence of AI coding agents, an era where AI generates code has arrived.  
However, cases are increasing where AI-generated code doesn't match project context,  
or even when error information is provided, corrections spiral into a quagmire.

Common responses to this situation:
- "AI is still immature, so it can't be helped"
- "The prompt is bad"
- "It will be solved when the model evolves"

However, **as long as you rely on "personal intuition (gut feeling)" and "prompt magic (luck)", project productivity remains hit-or-miss and doesn't scale.**

### Design Philosophy: Engineering Control Over Information Opacity

This framework's perspective:

> Rather than demanding perfection from AI,  
> place "information opacity between AI and humans" under engineering control

**Definition of Misreasoning (Mis-inference):**  
Not errors in the model's learning process, but **errors in context selection during the inference phase**.  
In other words, the phenomenon where AI selects knowledge unsuitable for the project's context during reasoning.

This perspective enables you to explain  
"what information was provided to AI that led to this result"  
not through personal intuition but **structurally**.

### 9-Axis Diagnostic Framework

AI-prone misreasoning areas are classified into the following **9 evaluation axes**.  
(The 9 axes are intentionally not independent—they're structured to influence each other.)

#### **Practice Layer (Community/Documentation/Practice)**

1. **Community Consistency**  
   Variations in community practices that cause AI to misselect context  
   *Example: Confusion between .NET and .NET Framework, ASP.NET MVC vs Minimal APIs factions*

2. **Documentation Consistency**  
   Inconsistencies between official and unofficial documentation that mislead AI's knowledge reference  
   *Example: Old API documentation versions, deprecated writing styles in blog posts*

3. **Practice Consistency**  
   Fluctuations in practice (writing styles, conventions, structure) that confuse AI code generation  
   *Example: Differences in database migration procedures, variations in naming conventions*

#### **Dependency Layer (Dependency/API/Ecosystem)**

4. **Dependency Stability**  
   Update frequency and breaking changes in dependent libraries that destabilize AI reasoning  
   *Example: Node.js major version upgrades, Python package compatibility breaks*

5. **API Consistency**  
   Lack of API consistency that induces AI's incorrect invocations  
   *Example: Same functionality implemented in multiple ways, different parameter orders*

6. **Ecosystem Consistency**  
   Differences between frameworks and toolchains that confuse AI reasoning  
   *Example: Build tool choices (Maven/Gradle), test framework options (Jest/Vitest)*

#### **Semantic Layer (Static/Runtime/Core)**

7. **Static Semantic Service**  
   Richness and consistency of semantics AI can reference at compile time  
   *Example: Type systems, AST, static analysis APIs (Roslyn, tsserver, etc.)*

8. **Runtime Semantic Service**  
   Stability and determinism of semantics observable at runtime  
   *Example: Exceptions, dynamic types, presence/absence of runtime type information*

9. **Core Semantic Consistency**  
   Consistency of the language specification itself  
   *Example: Semantic fluctuations due to historical context, backward compatibility policies, cultural values*

### Real Example: C# Diagnostic Results

#### **Modern C# (.NET 8 + ASP.NET Core Minimal APIs + EF Core)**

**Misreasoning Points in Community Consistency:**
- ".NET Framework (legacy)" and ".NET (current: .NET 6+)" contexts easily get mixed
- Even in ASP.NET Core, many factions exist: "MVC / Razor Pages / Minimal APIs / gRPC / Worker"
- While there are standards for DI, logging, configuration, HTTP, ORM, field preferences diverge

**Countermeasures to Provide AI:**
- Explicitly specify targets (.NET 8, ASP.NET Core Minimal APIs, EF Core, xUnit, etc.)
- Also specify "factions NOT adopted"
- Fix "adopted libraries and patterns" for DI, configuration, logging in written form

#### **Legacy C# (.NET Framework 4.8.1 + EF6)**

**Misreasoning Points in Community Consistency:**
- Gets pulled by general knowledge that ".NET is currently mainstream"
- Ignores .NET Framework 4.8.1 assumptions (App.config/Web.config, IIS, GAC, etc.)
- EF6 and EF Core knowledge easily gets mixed
- Assumes C# 8+ features (nullable reference types, switch expressions) as given

**Countermeasures to Provide AI:**
- Explicitly state upfront: "Strictly .NET Framework 4.8.1 / C# 7.3 / EF6 (.NET / EF Core prohibited)"
- Provide app type (WinForms/WPF/ASP.NET MVC, etc.) as fixed information
- Document adoption patterns (presence/absence of DI, Repository pattern, etc.)

**Key Insight:**  
The more detailed project-specific context you provide, the more detailed the diagnosis and the more practical the countermeasures.

### Design Emphasizing Long-Term Stability

Why this framework is powerful:

- **Project-independent**: Not dependent on specific tech stacks
- **Language-independent**: Uniformly evaluates static and dynamic languages
- **AI model-independent**: Not dependent on specific AI models
- **Long-term stability emphasis**: Prioritizes principles over rapidly evolving concrete tools

---

## 🧠 **About This Framework**

A framework developed to systematically explain AI coding stability  
from the perspectives of technology, language specifications, AI models, and project structure.

**Author:** Masaki Honda  
(Technical consultant, 40+ years of programming experience since MS-DOS era)

**License:** MIT License  
Free to use, modify, and distribute. See [LICENSE](https://github.com/mas-sdb/ai-coding-fixability/blob/main/LICENSE) for details.

**Initial public release:** 2025/12/29  

This framework is published as a thought experiment, and empirical validation is ongoing.  
Community extensions and improvements are welcome.
