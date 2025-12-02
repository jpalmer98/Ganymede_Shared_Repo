# Orchestration Prompt: Digital Product Demo Use Case Extraction

## Instructions

You are running a modular prompt chain for use case extraction and requirements synthesis from new product demo artifacts.

**Folder Contents:**
- The folder contains a sequence of 3 reference artifacts:
  - `artifact1.ipynb` (Jupyter Notebook implementing functionality)
  - `artifact2_techtransition.md` (Technical Transition/Background/Implementation document)
  - `artifact3_transcript.md` (Demo Transcript/user experience walkthrough)

- The folder contains a sequence of 4 prompts, each in its own `.md` file. Each prompt must be processed in order, using the latest output from the previous step.

## Workflow Steps

1. **Step 1 (Summary Extraction):**  
   Use `structured_summary.md` to produce a structured summary artifact from the input files.

2. **Step 2 (Requirements Q&A):**  
   Use `requirements_questions.md` to synthesize a requirements analysis by answering the 5 business analysis questions.

3. **Step 3 (Use Case Name Selection/Workflow Decomposition):**  
After completing Step 2 and providing the requirements analysis to the user, prompt:  
   - "Please provide the list of Use Case Names and a short description for each. This will ensure workflows are assigned to the correct Use Case artifacts."  
Wait for user input, then proceed.

4. **Step 4 (Use Case Generation):**  
   Use `generate_usecases.md` to create one markdown file per Use Case, referencing the summary and requirements analysis.

## Final Output

- For each workflow step, process the relevant prompt using the previously generated output(s) and reference artifacts.
- Output each step's result as a discrete `.md` file.
- At the completion of the workflow, you will have one `.md` file for each step, plus one `.md` file per Use Case.

**Do not pause for human review between steps except when prompting the user for Use Case Names/descriptions prior to Step 4.**

---
