
# Research & Discovery SOP

## Steps to Run Prompt Chain

### 1. Copy the content of 'orch-discovery.md'
- Copy the content of 'orch-discovery.md' from this Repository, located in the SOP folder.
  
### 2. Start new Copilot Chat Session
- Paste the content of 'orch-discovery.md' into the chat.
- Insert the exact file names of the Artifacts in the designated section of the orchestrating prompt.
  
### 3. Attach the following files
- Select **'Attach'**, then **'Files, Folders, Symbols'**
- Select this Repository **'jpalmer98/Ganymede_Shared_Repo'**
- Attach **'ResearchDiscovery'** folder
- Select **'Attach'**, then **'Files, Folders, Symbols'**
- Select this Repository
- Attach **'IMEA.Artifacts'**
  
### 4. Submit

## How It Works

<img width="7320" height="1860" alt="Research and Discovery" src="https://github.com/user-attachments/assets/0bf16f06-b4dd-4c59-9d46-f8f74bc0e9d5" />

The Input outlined here should produce one markdown (.md) file.
- The first two outputs will be titled `Questions.md` and `Definitions.md`. These files will be created as markdown files and viewable in the Workbench with a one sentece description of the purpose of the file.
- After Questions and/or Terms and Definitions are drafted in the chat, you will have the opportunity to refine and approve the list of questions, at which point they will be added to the `Questions.md` or `Definitions.md` file.

### 1. Input

- Attach the `IMEA.Artifacts` folder which contains the source material (Demo Transcript, Technical Transition Doc, and Sample Code).
- Attach the BID_Requirements_Team_Repo/ResearchDiscover/ folder which contains the prompt files for Question discovery.
- Copy & Paste `orch-discovery.md` which contains the orchestrating prompt.

### 2. Automated Step Processing

The chain inititates a collaborative refinement session, using the prompt files in `ResearchDiscovery/`:

1. `WorkflowDiscovery.md` - Output: Current State Workflow & Future State Workflow
3. `EdgeCasesDiscovery.md` - Output: Edge Cases
4. `DataDiscovery.md` - Output: Data Sources & Data Elemenets

For each step:
- Copilot will ingest the artifacts and prompts.
- Copilot will ask the user to select a category to proceed.
- Copilot will produce tables or lists with information pulled from artifacts. Each row will include a citation for tracking.
- Copilot will pull the questions and/or definitions into a numbered list.
- The user can accept, refine, or reject questions/definitions, by number, and Copilot will update the list.
- When the user approves a list of questions/definitions, they are added to the `Questions.md` file or `Definitions.md` file, respectively.

## Usage Instructions

### For Interactive Analysis

**Recommended Approach: Focus on one Discovery Stage at a time with approval**

1. **Initial Request**: Follow the instructions above to kick off the Interactive Discovery Session.

2. **Agent starts with creating three markdown files** (Questions.md, EdgeCases.md, and Defintions.md).

3. **User reviews markdown files to ensure each is correctly created and populated**

4. **User approves**: Give explicit approval to process

```
Looks good! Proceed.
```
OR request changes:
```
Can you add in the attached Artifact?
```

5. **Agent proceeds to Discovery Category selection**

6. **User selects Category**: Simply respond with the number or full name of the category you wish to work on.

```
1.
```
OR
```
Workflow Discovery
```

7. Proceed through each Discovery Category

**Benefits of this approach:**
- Better quality control - catch issues early
- Reduced token usage per conversation
- User stays engaged and informed
- Easier to manage and review
- Can course-correct before issues cascade
