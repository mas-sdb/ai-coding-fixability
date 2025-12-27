# AI Coding Assistant Diagnostic Framework  
## Principles for Long-Term Stability

## **1. Position as a tool for assisting AI, not evaluating languages**
- This framework does not determine language superiority.  
- Its purpose is to identify structures prone to mis-inference and  
  **maximize AI coding assistant reasoning accuracy**.  
- Language values and history are treated solely as background context.

---

## **2. Respect language values; exclude them from evaluation axes**
- Each language embodies its own values (backward compatibility, evolution speed, pragmatism, etc.).  
- Treat these not as "good or bad," but as **contextual factors that shape semantic information structures**.  
- Maintain **neutrality and objectivity** by excluding value judgments from diagnostic axes.

---

## **3. Focus on structural inconsistencies; avoid superiority rankings**
- Do not compare language or ecosystem maturity.  
- Focus on **structural inconsistencies** that make AI prone to mis-inference.  
- Limit output to mis-inference points and mitigations, excluding value judgments.

---

## **4. Maintain consistent abstraction for long-term diagnostic consistency**
- Diagnostic results should be comparable across projects.  
- Avoid excessive specificity; evaluate using **9 axes with uniform abstraction**.  
- Fix diagnostic granularity to ensure long-term stability and reproducibility.

---

## **5. Avoid over-structuring; prioritize framework robustness**
- Excessive subdivision of diagnostic axes reduces resilience to language and version differences.  
- Intentionally maintain abstraction to ensure **cross-language and cross-version resilience**.  
- Explicitly adopt **"know when to stop"** as a design principle.

---

## **6. Organize semantics by observability to absorb language differences**
- To treat static and dynamic languages uniformly,  
  define **Static / Runtime / Core** axes by observability:  
  - **Static**: Semantics observable at implementation-time (compile-time or static analysis)  
  - **Runtime**: Semantics observable at execution-time  
  - **Core**: Semantics fixed by language specifications  
- This enables handling **mis-inference structures in a common framework**  
  while absorbing differences in language history and design philosophy.

---

## **7. Minimize output format to reduce AI response variability**
- Limit output to two components:  
  - AI mis-inference points  
  - Recommended mitigations  
- This prevents AI from diverging into general discussions or value judgments,  
  maintaining **diagnostic stability and comparability**.

---

## **8. Treat project-specific context as overridable configuration**
- Separate general language diagnostics from project-specific diagnostics.  
- Use project-specific information (dependencies, architecture, versions)  
  **to refine diagnostics without altering diagnostic axes**.  
- This preserves long-term framework consistency.

---

## **9. Treat version specification as additional refinement information**
- Diagnostics can be refined when version information is provided,  
  but **maintain a structure that works without it**.  
- This ensures resilience to input variations and supports long-term operation.
