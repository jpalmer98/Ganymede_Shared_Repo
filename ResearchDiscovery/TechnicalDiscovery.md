## 3. Technical Discovery
### Instructions
- Produce a table for each of the Required Outputs below.
- Each table should contain information pulled directly from the source file.
- After the table(s) is produced, prompt the user to refine the Clarifying Questions. If the user accepts, follow the instructions provided in Step 3 above.

### Required Outputs
- Based on the attached Source File, create a table titled 'Data Sources' with the following columns:
  - Columns: Data Source / Application | Usage Description | Produced / Consumed Data | Limitation / Gap | Clarifying Question(s) | Citations
- Based on the attached Source File, create a table titled 'Data Elements' with the following columns:
  - Columns: Data Element | Usage Description | Produced / Consumed Data | Limitation / Gap | Clarifying Question(s) | Citations
- Based on the attached Source File, create a table titled 'Technical Logic' with the following columns:
  - Columns: Step # | Component | Input(s) | Transformation | Output | Clarifying Question | Citations

### Data Source vs Data Element Differentiation Rules:
- Data Source / Application: A system, service, repository, platform, tool, database, API, queue, or external partner interface that produces, stores, or consumes sets of data. Represents a macro container or integration endpoint (e.g., “Claims Adjudication System”, “Document Repository API”).
- Data Element: An atomic field, derived field, code, identifier, flag, status, value range, or calculable metric (e.g., “Claim Priority”, “EP Code”, “Submission Timestamp”).
- Avoid Duplication:
  - Do NOT list a Data Source name in the Data Elements table.
  - Every Data Element row should be mappable to at least one Data Source (cite the source file excerpt mentioning both when available).
  - If a Data Element is derived from multiple Data Sources, cite all relevant excerpts separated by semicolons.
  - Produced / Consumed Data (in Data Sources table): List only categories or representative examples of Data Elements (not whole records) referenced in the source; each should appear (or be planned to appear) in the Data Elements table.

### Data Source Guardrails

Data Source: Any origin of data (system of record, operational system, transactional store, API/service, file feed, partner interface) that supplies information consumed directly or indirectly by the product's workflows, logic, validation, reporting, or governance controls.

Not all data references are distinct sources; some are derivatives or mirrors of an authoritative sources.

---

Treat something as a distinct Data Source when MOST of these are true:
- Authoritativeness: It is the recognized or contractual “system of record” for one or more data domains consumed.
- Direct Integration: The product must connect (API, feed, event, query) rather than receiving data via an existing intermediary already cataloged.
- Structural Dependency: Schema, format, or refresh cadence affects workflow correctness or SLA.
- Change Sensitivity: Source modifications (schema, field meaning, cadence) require impact assessment.
- Governance Relevance: Ownership, access controls, retention, or privacy classification must be tracked.
- Quality Risk: Data defects could propagate into user-facing decisions, compliance outputs, or downstream systems.
- If most do NOT apply, represent it as a derived dataset or internal transformation instead of a primary source.

---

Do NOT consider as a data source if:
- It is only a cached, indexed, or materialized view of another already listed authoritative source.
- It is purely a transformation layer (ETL step, enrichment join) without independent ownership.
- It is a temporary test/mock feed not slated for production.
- It is a duplicate path (e.g., same API reachable through a gateway already cataloged).
- It supplies non-critical, low-risk reference values easily embedded/static (e.g., a short static code list under tight version control).
- Its data is fully supplanted by a higher-fidelity source planned for MVP (i.e., it will be retired immediately).

---

### Data Element Guardrails:

Data Element: An atomic, discrete unit of information (field, attribute, flag, KPI, status code, identifier, timestamp, calculated value, derived metric) that:
- Is stored, transmitted, displayed, validated, transformed, or audited
- Has a defined meaning, datatype, permissible values, and lifecycle (creation, update, retirement)
- Supports workflow logic, decisions, reporting, governance, analytics, integration, or user experience

Not every entry in a payload is a distinct Data Element; placeholders, purely decorative UI labels, and transient computed artifacts may not warrant cataloging.

---

Register a Data Element when MOST of these apply:
1. Business Significance: Drives a decision, rule, eligibility, visibility, or compliance requirement.
2. Reuse: Appears across multiple workflows, services, APIs, or reports.
3. Governance Need: Requires stewardship, access control, retention, provenance, or audit.
4. Validation Rules: Has non-trivial constraints (format, range, enumerations, referential integrity).
5. Sensitivity: May contain regulated, personal, confidential, or security-relevant data.
6. Quality Impact: Incorrect, missing, or stale values can cause adverse outcomes.
7. Lineage Complexity: Derived from or influences other elements (calculations, transformations).
8. Change Risk: Meaning, format, or derivation changes require coordinated updates.

If most do NOT apply, consider treating it as a transient UI-only value or part of a composite structure rather than a cataloged element.

---

Do not register as a distinct Data Element if:
- It is purely a formatting artifact (e.g., display-only concatenation with no downstream logic).
- It duplicates an existing element under a different label (alias without semantic difference).
- It is an ephemeral computation used only within one internal processing step and not persisted or exposed.
- It is a static literal value (e.g., hard-coded label) unlikely to change or require governance.
- It is solely a transport wrapper (e.g., pagination token) with no business meaning.
- It is replaced immediately by a standardized canonical element at ingestion.

### Technical Logic Table - Branching & Conditional Flow Handling:
- One row = one atomic transformation or decision evaluation.
- Step #:
  - Use integers for linear sequence (1, 2, 3).
  - For branches emerging from a prior step, append a letter (e.g., 3a, 3b) for mutually exclusive paths.
  - For converging steps after branches merge, resume next integer (e.g., 4).
- Conditional Transformations:
  - Place the condition predicate at the start of the Transformation cell in square brackets (e.g., “[If claim priority = High] Recalculate allocation score…”).
  - If multiple conditions apply, separate them with “;” inside the brackets.
- Loops / Iteration:
  - Indicate iteration in Transformation cell using “[Loop: each Document]”.
  - Reuse Step # with letters if the loop contains conditional sub‑branches (e.g., 5a, 5b inside a loop).
- Parallel Processes:
  - If two operations occur in parallel, use the same base Step # with different letters (e.g., 6a, 6b) and state “[Parallel]” in Transformation.
- Clarifying Question:
  - Each branch row must have a branch‑specific clarifying question unless source explicitly resolves all logic.
  - Do not combine multiple branch conditions in one row (“double‑barreled”); split rows instead.

Technical Logic Guardrails:

Techinical Logic: An atomic, deterministic (or intentionally non‑deterministic) operation, decision, orchestration action, transformation, validation, routing, persistence, security enforcement, or error handling activity performed by a system component that:
- Consumes one or more inputs (data elements, events, states, external responses)
- Applies defined rules, algorithms, mappings, or conditions
- Produces an output (state change, data structure, event, decision, side effect)
- Advances a workflow toward its intended outcome or guards against invalid progression

A “step” should be granular enough to be testable and traceable (single responsibility), but not so fine-grained that trivial language-level statements (e.g., variable assignment) are cataloged.

---

Inclusion Criteria (Treat as a Technical Logic Step when MOST apply)

1. Business or System Significance: Drives a rule, decision, compliance, security, or state transition.
2. Explicit Inputs & Outputs: Clearly identifiable input set and resultant output/state.
3. Non-Trivial Transformation: More than a simple pass-through; includes mapping, calculation, validation, enrichment, branching, or orchestration.
4. Failure Modes: Has identifiable error/exception or timeout scenarios needing handling.
5. Testability: Can be unit/integration tested with measurable acceptance criteria.
6. Traceability Need: Stakeholders may audit, debug, optimize, or govern its behavior.
7. Reuse / Centralization: Logic appears across multiple flows/components or should be centralized to avoid duplication.

---

Exclusion Criteria (If ANY True → Do NOT Catalog Separately)

- Pure framework boilerplate (dependency injection wiring, trivial getters/setters).
- Simple data pass-through with zero transformation or validation.
- UI-only presentation formatting (e.g., styling decisions) with no functional impact.
- One-off inline code that will not be reused nor audited and poses negligible risk.
- Temporary development/testing scaffolding (stubs, mocks, placeholder branches).
- Logic wholly encapsulated by a standard library call without custom parameters of significance.
