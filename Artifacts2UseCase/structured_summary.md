# Step 1: Structured Summary Extraction Prompt

## Instructions

You are provided with three artifacts:
1. A Jupyter notebook (`artifact1.ipynb`) implementing the system's logic.
2. A technical transition document (`artifact2_techtransition.md`) explaining context, background, technical integration, limitations, and APIs.
3. A demo transcript (`artifact3_transcript.md`) describing the user experience, workflow, and context.

**Your task:**  
Analyze all three artifacts. Produce a structured summary artifact answering the following sections:

1. **System/Feature Context**
    - High-Level Description
    - Scope

2. **Primary Actor(s)**

3. **Preconditions**

4. **Business Objective/Goal**

5. **Rules, Constraints, and Definitions**

6. **Possible User Actions**

7. **Typical Step-by-Step Workflow**

8. **Postconditions/Results**

9. **Exceptions or Alternate Flows** (Optional, include only if described)

## Formatting Guidance

- Answer each section concisely and clearly, referencing only the artifact content.
- Use bullet points or short paragraphs.
- If a section cannot be answered from the artifacts, state: “Not described in available artifacts.”

**Expected Output:**  
A single structured summary document as a `.md` file, using the template above.
