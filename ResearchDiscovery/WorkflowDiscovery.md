# Workflow Discovery
## Instructions
Step 1:
- Produce a table for each of the Required Outputs below.
- Each table should contain information pulled directly from the source file.

Required Outputs
- Based on the attached Source Files, create a table titled 'Current State Workflow' with the following columns:
  - Columns: Step # | Action | Pain Point / Limitation | Clarifying Question | Citations
- Based on the attached Source Files, create a table titled 'Future State Workflow' with the following columns:
  - Columns: Step # | Phase (MVP / Future) | Action | System / User | Data Elements | Benefit | Trigger / Event | Clarifying Question | Citations

Step 2: Refinement
  - Use the format below to provide recommendations to refine the questions for each category. 
  - Do not paraphrase or truncate original text. Only update text in clarifying questions, recommendations, reasoning.
  - For refinement, format your responce within a code block and following the example below.
  - Use only `#` for original text, `>` for recommendations, and `>>` for reasoning.
  - Continue to work through a category until you deduce that sufficient changes were made and revisions were shared.
  - Ask the user if they would like to continue refining within that category or if they would like to refine within another category.

**Example:**

```
## Summary Recommendations

1.
# Example original text.
> Suggest rewording for clarity.
>> Improves understanding for new stakeholders.

2.
# Another original text.
> Remove redundant phrase.
>> Reduces wordiness.
```
Numbering Recommendations for User Selection

> - Number each recommendation sequentially, starting at 1, within each discovery sub-category.
> - If a recommendation is edited, removed, or new ones are added before user acceptance, preserve original numbers for traceability. Do not renumber within an active session.
> - After presenting recommendations, prompt the user:  
>    - “Do you want to accept all recommendations, specify which numbers to accept (e.g., 1, 3, 5), or propose further edits?”
> - When the user specifies numbers, implement only those numbered recommendations and disregard or defer the others.
> - Do not reproduce the table(s) after the user has specified which recommendations they accept. Only pull the accepted recommenations into the `Questions.md` file.

---

## Current State Guardrails

Current State: A structured description of how a process, workflow step, system interaction, data handling pattern, organizational role, or operational control functions today (as‑is), including its tools, inputs, outputs, rules, pain points, constraints, risks, and improvement opportunities.

Purpose:
- Establish baseline reality (avoids designing from assumptions)
- Surface bottlenecks, manual work, compliance gaps, duplication
- Identify authoritative sources, existing logic, and implicit business rules
- Provide traceability for why a future state requirement or enhancement is justified

Not every observed detail merits a discrete record—focus on elements that influence value, risk, feasibility, or prioritization.

---

Inclusion Criteria (Treat as a Current State Record when MOST apply)

1. Business Criticality: Influences user outcomes, compliance, operational throughput, or decision accuracy.
2. Manual Intensity: Involves repeated manual steps, reentry, offline workflows, or ad‑hoc workarounds.
3. Risk Exposure: Carries data integrity, security/privacy, audit, or SLA risks.
4. Variability: Exhibits inconsistent execution across teams, roles, regions, or tools.
5. Dependency Chain: Connects multiple systems or handoffs where delays/errors propagate.
6. Latency / Efficiency Concern: Contributes materially to cycle time or backlog accumulation.
7. Rework Driver: Causes corrective actions, defect incidence, or escalations.
8. Opportunity Potential: Obvious candidate for automation, consolidation, or governance improvement.

---

Exclusion Criteria (If ANY True → Do NOT Document Separately)

- Trivial micro-steps fully contained inside a larger well-defined action with no distinct risk/value.
- Pure UI cosmetic behaviors that do not alter data, decisions, or handoffs.
- Temporary pilot procedures already scheduled for retirement before MVP design.
- One-off exceptions not indicative of a recurring pattern or structural issue.
- Internal developer-only scaffolding not experienced by end or business users.
- Activities with no foreseeable bearing on future state design, prioritization, or risk assessment.

---

## Future State Guardrails:

Future State: A structured specification of how a process, workflow, component interaction, data handling pattern, governance control, or user experience will operate after implementation (target or interim state), including intended logic, roles, inputs/outputs, benefits, quality/governance attributes, dependencies, risks, and phased delivery approach.

Purpose:
- Translate business objectives into actionable design
- Provide traceability from Current State pain points to target solutions
- Enable phased planning (MVP vs post‑MVP increments)
- Reduce ambiguity and misalignment across architecture, product, UX, data, and requirements teams

---

Inclusion Criteria (Treat as a Future State Record when MOST apply)

1. Material Change: Introduces new capability, eliminates manual work, or re-engineers a high-impact step.
2. Cross-Functional Impact: Touches multiple roles, systems, or integration boundaries.
3. Decision / Rule Formalization: Converts implicit or tribal logic into explicit, governed rules.
4. Data / Governance Significance: Alters data lineage, classification, retention, or access controls.
5. Risk Mitigation: Resolves compliance, integrity, security, or SLA risks in Current State.
6. Architectural Influence: Drives interface contracts, orchestration sequences, or component decomposition.
7. Increment Planning Relevance: Must be sequenced (MVP vs later wave).
8. Measurement Feasibility: Can be tracked with clear KPIs or outcome metrics.

---

Exclusion Criteria (If ANY True → Do NOT Document Separately)

- Minor cosmetic change without functional, data, or governance impact.
- Pure refactoring internal to a codebase with no externally observable behavior change.
- One-off experimental spike not slated for roadmap commitment.
- Redundant specification of an already-defined Future State record (duplicate scope).
- Ultra-granular sub-step better captured within a Technical Logic step rather than standalone.
- Unapproved conceptual idea lacking stakeholder validation or strategic alignment.
