# Edge Case Discovery
## Instructions
Step 1:
- Produce a table for the Required Output below.
- The table should contain information pulled directly from the source file.

Required Outputs
- Based on the attached Source File, create a table titled 'Edge Cases' with the following columns:
  - Columns: Edge Case | Trigger / Condition | Limitation | Clarifying Question | Citations

Step 2: Refinement

- Use the format below to provide recommendations to refine the questions for each category.
  - Do not paraphrase or truncate original text only update text in clarifying questions, recommendations, reasoning.
- For refinement, format your responce within a code block and following the example below.
- Use only `#` for original text, `>` for recommendations, and `>>` for reasoning.
- Continue to work through a category until you deduce that sufficient changes were made and revisions were shared.
- Ask the user if they would like to continue refining within that category or if they would like to refine within another category.

Example:

```
Summary Recommendations

1.
# Example original text.
> Suggest rewording for clarity.
>> Improves understanding for new stakeholders.

2.
# Another original text.
> Remove redundant phrase.
>> Reduces wordiness. Numbering Recommendations for User Selection
```

- Number each recommendation sequentially, starting at 1, within each discovery sub-category.
- If a recommendation is edited, removed, or new ones are added before user acceptance, preserve original numbers for traceability. Do not renumber within an active session.
- After presenting recommendations, prompt the user:
- “Do you want to accept all recommendations, specify which numbers to accept (e.g., 1, 3, 5), or propose further edits?”
- When the user specifies numbers, implement only those numbered recommendations and disregard or defer the others.
- Do not reproduce the table(s) after the user has specified which recommendations they accept. Only pull the accepted recommenations into the Questions.md file.

## Edge Case Guardrails
- Edge Case: A low-frequency, atypical, boudary, degraded, concurrent, or anomalous condition that:
  - Causes a workflow to diverge from the standard or "happy" path
  - Risks incorrect outcomes, data integrity issues, security exposure, operational inefficiency, or user confusion if unhandled
  - Requires explicit handling logic, fallback behavior, validation, messaging, or constraint enforcement
- Treat a scenario as an Edge Case when:
  - Atypical Frequency: Occurs rarely relative to standard operational volume.
  - Boundary / Extremes: Involves min/max values, null/empty sets, overflows, unusual date ranges, or timeouts.
  - Degraded State: Arises when a dependent service, data source, or component is slow, partial, unavailable, or returns inconsistent formats.
  - Concurrency / Race: Emerges only under parallel actions (simultaneous edits, updates, submissions).
  - Anomalous Input / State: Input violates format, schema, domain rules, or arrives in unexpected sequence.
  - Timing Sensitivity: Relates to cutoff boundaries (end of period, daylight saving shift, expiration rollover).
  - Security / Access Edge: Involves permission transitions, orphaned ownership, revoked roles mid-operation.
  - Environmental Variant: Different behavior under browser, device, locale, region, or infrastructure variance.
  - Cascading Impact: Can propagate failure to multiple downstream steps if not contained.
- Do NOT label as Edge Case if:
  - It is a documented, high-frequency alternate business rule or standard variation.
  - It represents normal user progression through approved optional paths.
  - It is purely a future enhancement (added value rather than risk mitigation).
  - The “scenario” is speculative with no credible trigger source, data evidence, or stakeholder validation.
  - It duplicates another already defined edge with only superficial difference (e.g., formatting vs casing when both derive from the same validation rule).
