# Evaluation Samples

This directory contains example evaluations using the Language Ecosystem Evaluation Model framework.

## Purpose

These samples demonstrate:
- How to apply the 9-axis evaluation model
  - The [diagnostic_prompt](../diagnostic_prompt.md) used in these samples converts the model's abstract 9 axes into names tailored for actual project diagnostics.
  - For details on this naming conversion, refer to the [Model Usage Guide](../model-usage-guide.md).
- Different language ecosystems and use cases
- Structural observations without quality rankings
- Practical mitigation strategies for AI coding
- Impact of context depth (minimal vs. detailed) on evaluation

## Structure

- Each file represents one language/version/use case evaluation
- Format: Input parameters → Evaluation results by axis
- Focus: Structural observations that affect AI's correction loop
- Language: English samples here, 日本語サンプルは [ja/](./ja/) へ

## Available Samples

### Python
- [python-001.md](./python-001.md) - Python (minimal context - language name only)
- [python-002.md](./python-002.md) - Python 3.12 + LangChain + FastAPI on Azure

### Java
- [java-001.md](./java-001.md) - Java (minimal context - language name only)
- [java-002.md](./java-002.md) - Java 8 + Spring Boot + Hibernate on Azure App Service

### C#
- [csharp-001.md](./csharp-001.md) - C# (minimal context - language name only)
- [csharp-002.md](./csharp-002.md) - C# 7.3 + .NET Framework 4.8.1 + Entity Framework 6

## Sample Types

### Minimal Context Samples (*-001.md)
Demonstrate evaluation with only language name provided:
- Shows how AI struggles with ecosystem diversity
- Highlights importance of explicit constraints
- Reveals common mis-inference patterns across versions/frameworks

### Detailed Context Samples (*-002.md)
Demonstrate evaluation with full project context:
- Specific versions, frameworks, and deployment targets
- Detailed mitigation strategies for each axis
- Real-world complexity and constraint handling

## Using These Samples

1. **Read the input parameters** - Understand the context depth
2. **Review axis-by-axis analysis** - See how each axis affects AI coding
3. **Compare minimal vs. detailed** - Observe how context improves evaluation
4. **Check mitigation strategies** - Learn how to support AI better
5. **Adapt to your project** - Apply similar analysis to your ecosystem

## Key Insights from Samples

### Minimal Context Challenges
- AI easily mixes incompatible framework versions
- Community practice diversity causes incorrect assumptions
- Lack of constraints leads to cross-generation pattern mixing

### Detailed Context Benefits
- Specific version constraints prevent incompatible suggestions
- Explicit framework choices eliminate ambiguity
- Deployment target clarity enables environment-appropriate recommendations

## Note on Maintenance

This is a snapshot repository. Sample quality and relevance may vary over time as languages and ecosystems evolve. Consider these as starting points rather than definitive analyses.

---

## Related Resources

- [Framework Documentation](../model.md)
- [Design Principles](../philosophy.md)
- [日本語サンプル](./ja/)

**Keywords:**  
evaluation samples, AI coding, language ecosystems, Python, Java, C#, context depth, mis-inference patterns
