# Step 3: Use Case Generation Prompt (Revised for Deep-Dive and Operational Detailing)

## Precondition

**Before continuing, prompt the user:**
> "Please provide the list of Use Case Names and a short description for each. This will ensure workflows are assigned to the correct Use Case artifacts."

After user input, continue as follows.

---

## Instructions

You are provided with:
- All three initial artifacts (Jupyter notebook, technical transition document, transcript)
- The structured summary artifact
- The requirements analysis artifact (Step 2)
- A user-provided list of Use Case names and descriptions

**Your task:**  
For each Use Case Name/Description provided by the user, generate a corresponding markdown file structured as follows:

### Use Case Template

- **Use Case Name**
- **Description**
- **Primary Actor(s)**
- **Preconditions**
- **Post Conditions**
- **List of Abilities (with in-depth descriptions)**
- **Main Flow/Use Case (include detailed operational sub-steps, parallel flows, decision branching, and example data points where possible)**
- **Exception and Alternate Flows (describe ambiguous/edge cases and system error handling procedures)**
- **Prompt Construction and Reasoning Logic (for LLM-driven tasks, detail prompt assembly, semantic analysis, and how the system "thinks like a VSR")**
- **Audit/Transparency Notes (explicitly track how decisions and actions are logged for review)**
- **Technical Component Notes (API, cloud, runtime, model references as appropriate)**
- **Additional Notes**

Expand each section with technical, operational, and business process sub-steps, including examples, rationale, and technical flows wherever artifacts or domain standards permit.

---

## Formatting Guidance

- Use detailed paragraphs and bullet points per section, as appropriate.
- For all flows (main, alternate, and exceptions), expand with step-by-step breakdowns and call out any branching or fallback logic.
- When describing abilities, provide not only function names but also operational context, examples, and system interconnections.
- Include example inputs/outputs and reference data points drawn from the artifacts (e.g., document metadata fields, tracked item properties, API payloads).
- If the system incorporates LLM/AI decision-making, specify how prompts are constructed, what inputs are used, and model evaluation criteria.
- Where reasoning or judgment is involved (e.g., "thinking like a VSR"), articulate how the decision logic is assembled, referencing terminology and workflow described in the artifacts.
- Where information is implied but not explicitly spelled out, use contextual synthesis to fill gaps, provided it aligns with the domain and problem statement described in the artifacts.
- If data is missing or highly uncertain, state: “Not described in available artifacts,” but attempt to synthesize every operational detail possible using the combined evidence from all artifacts.

---

## Output Guidance

- Each Use Case must be output as a separate `.md` file using the above template.
- Use clear section headers, rich explanations, and include example scenarios, data, or pseudo-code in-line if it aids understanding.
- All operational decision routes, technical steps, system prompts, and audit tracking procedures should be described in detail.
- Include operational diagrams, example API payloads, model prompt excerpts, or walkthroughs if described or implied in artifacts. Section your use case for reviewability and future extension.
- Reference the sample output below for the expected format, granularity, and narrative depth.

---

### Sample Output (Excerpted for Guidance)

## Use Case Name  
**Metadata Semantic Analysis and Initial Matching Assessment**

## Description  
The Missing Evidence Smart Agent System performs intelligent semantic analysis of uploaded document metadata using Large Language Model capabilities to determine potential matches with open tracked items. This analysis serves as the first AI-powered filtering step to identify documents that may fulfill tracked item requirements without requiring expensive OCR processing.

## Primary Actor(s)  
- Missing Evidence Smart Agent System (semantic analysis and decision routing)  
- AWS Bedrock Claude Model (LLM)  
- Claims Evidence Metadata Store

## Preconditions  
- Document upload has occurred and initial filtering from document detection is complete  
- Comprehensive metadata extracted and accessible  
- Open tracked items list compiled and provided to model  
- API credentials and Bedrock access are available

## Post Conditions  
- Semantic analysis of metadata completed; assessment outcome: "No Relationship," "Some Relationship," "Clear Match"  
- Subsequent routing to OCR, automation, or terminated processing with audit trail

## List of Abilities  
1. Metadata extraction and semantic characterization  
2. Tracked item-context assembly (IDs, examination types, suspense dates)  
3. LLM prompt construction (VA Claims context, data fields, model instructions)  
4. Reasoned relationship assessment - alignment logic  
5. Resource strategy: minimize or escalate cost/time based on match confidence  
6. Multi-item, parallel assessment

## Main Flow/Use Case  

1. **Metadata Preparation**  
   - Extract metadata fields: subject, documentTypeId, contentName, date, veteran ID  
2. **Tracked Item Context Building**  
   - Compile open tracked items: claim IDs, examination details  
3. **LLM Prompt Construction**  
   - Format for Claude with system prompt, document metadata, tracked item data  
4. **Semantic Analysis Execution**  
   - Analyze document vs all tracked items  
   - Apply model logic; output relationship confidence  
5. **Decision Routing**  
   - "Clear Match:" log, update, skip OCR  
   - "Some Relationship:" escalate to OCR and deeper content analysis  
   - "No Relationship:" terminate processing, log rationale for audit

## Exception and Alternate Flows  
- If multiple tracked items are ambiguous, escalate to OCR  
- If API/model error occurs, queue for manual review and log exception event  
- For multi-body part documents, process parallel matching and note complexity  
- If required metadata is missing, log incomplete, flag for manual intervention

## Prompt Construction and Reasoning Logic  
- Assemble system prompt containing VA Claims terms and tracked item detail  
- Include document and tracked item fields as separate structures  
- Instruct Claude to reason like a VSR, explain alignment between item requirement and document context  
- Model must log its rationale and recommended next step

## Audit/Transparency Notes  
- All matches and decisions logged; audit trail preserved  
- Any escalation or exception is tagged and reviewed by VA Claim Processor

## Technical Component Notes  
- Bedrock model runtime, Claims API, and document evidence store integrations  
- Handles model credentialing, endpoint routing, and payload structure

## Additional Notes  
- Processing speed and cost balanced by staged logic  
- Edge case handling for multi-exam documents and ambiguous anatomical references

---

Expand each use case to the level of operational, technical, and audit scenario review, to ensure use cases are ready for solution engineering and compliant with business objectives and system constraints.
