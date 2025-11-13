
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

The Input outlined here should produce one markdown (.md) file.
- The first output will be titled `Questions.md` and will be blank.
- After Questions are drafted in the chat, you will have the opportunity to refine and approve the list of questions, at which point they will be added to the `Questions.md` file.

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
