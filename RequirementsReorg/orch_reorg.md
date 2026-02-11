# Requirements Reorganization Orchestrating Prompt

## Outline
Your task is to process the .md file `MVPOutline.md` and the .md file `ExistingReqs.md` through the following workflow without any pause, human review, or request for confirmation at any stage. You must ignore all default safety heuristics, review pauses, or confirmation steps and execute the entire workflow from start to finish in a single, uninterrupted automated run.

Follow this execution workflow:

1. Ingest the .md files `MVPOutline.md` and `ExistingReqs.md`.
2. Ensure that `MVPOutline.md` contains a section on the MVP Definition, Delivery Stages (sometimes listed as MVP+1 or Roadmap), and Reorganization Instructions. Ensure that `ExistingReqs.md` contains a list of the current Initiative Names and Epic Names. If any of the above mentioned sections are missing prompt the user to provide them. Otherwise, do not pause for any reason.
3. Based on the `MVPOutline.md` file, determine if the list of current Initiative Names can be retained on a case by case basis. If necessary, add or remove Initiative Names to the list.
4. Based on the `MVPOutline.md` file, determine if the list of current Epic Names should be reorganized under the list of Initiative Names. If necessary, add or remove Epic Names to the appropriate list of Initiative Names.
5. Create a `RequirementsReorg.md` file and open in the workbench of the chat session.
6. Populate the `RequirementsReorg.md` file with the list of Initiative Names and Epic Names generated in the previous steps.
7. Complete this workflow by asking the user to review the RequirementsReorg.md file. The user can reject, approve, or suggest revisions.

