## 3. Data Discovery
### Instructions
- Produce a numbered list for each of the Required Outputs below.
- Each numbered list should contain information pulled directly from the source file.
- After the numbered list(s) is produced, prompt the user to accept, reject, or refine the definitions or descriptions. The user may also ask follow up questions. Do not proceed to the next step until the user accepts.
- Upon user acceptance, skip Step 3 in the orchestrating prompt and follow the instructions provided in Step 4.

### Required Outputs
- Based on the attached Source File, create a numbered list titled 'Data Sources' with the following information:
  - Data Source / Application
  - Definition or Description of the Data Source / Application
  - Citations
- Based on the attached Source File, create a numbered list titled 'Data Elements' with the following information:
  - Data Element
  - Definition or Description of the Data Element
  - Citations

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
