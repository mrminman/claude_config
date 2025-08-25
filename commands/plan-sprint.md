---
allowed-tools: Bash(ls:*), Bash(mkdir:*), Bash(date:*), Powershell
description: Create a "sprint plan" for the next tasks to execute; this includes writing detailed tasks for test and code to be written.
---
# plan-sprint
Create a "sprint plan" for the next tasks to execute; this includes writing detailed tasks for test and code to be written.

## Usage
```
/plan-sprint <folder>
```

## Validate input
1. **Confirm folder name and TDD (or similar technical design document)**
   - The folder specified should be under `docs/` and contain a TDD. Check for similar names. If the folder is not found, or there is no TDD/technical/architectural doc in the foler, reply an error saying that the command must be called with an existing project folder and TDD

2. **Confirm that the input documents are valid**
   - Read the file(s) in the folder including the PRD/design doc and TDD
   - Ensure they contain valid details for planning engineering work

## Sprint Plan file name
1. **Ensure there are not outstanding work items already**
   - Check for a file called `docs/<feature slug>/sprint-plan.md`. 
   - If the sprint-plan file already exists: Check whether all the tasks/stories/epics are checked off already. Confirm that the corresponding stories/epics have a green check ✅ before them in the TDD (if not, add the green checks to the TDD).
   - If the existing sprint-plan is NOT all checked, off *stop* and ask the user whether they want to rewrite the sprint plan for the current epic, or execute the rest of the existing plan with `/execute-sprint-plan`
   - If the existing plan does not exist, was already completed, or if the user decided to overwrite the plan, create a new file called `docs/<feature-slug>/spring-plan.md`
   

## Check for related documents
- Check `docs` for other product documents, PRDs, design documents, or descriptions that are relevant, as well as other TDDs that may be relevant.

## Write the sprint plan
Explicitly **invoke the `feature-architect` agent** to write the sprint plan, with the following instructions:

Throughout the writing of the sprint plan, read relevant sections of code to understand context for what needs to be built and how the new features will integrate with code that already exists. You should rely heavily on context from the TDD and existing project source code as you plan the work.

0. **Read relevant documents**
    - Gain context by reading the PRD and TDD inside the `docs/<feature-slug>` folder

1. **Choose the next incomplete epic from the TDD and mark it in-progress**
    - Identify the next TDD without a ✅. Add an "in-progress" emoji ⚠️ before the title line of the epic and each of the incomplete/unstarted stories beneath it 

2. **Describe the epic and what should be possible by the end of the sprint**
    - Following agile principles, there should be user-facing functionality that is verifyable by the product champion at the end of the sprint. Add a section to the document describing 1) What a product champion will be able to do test the end of the sprint; 2) what data each feature will use (real data or mock data),  3) How they would test it (very brief user journey); 4) acceptance criteria of done-ness for each user-facing feature and 
    - It is OK if mock data is required to demonstrate the functionality of the epic being planned, but if that's the case then during this step edit the TDD to add stories in the current epic to generate mock data, and stories in future epics to capture when mock data is removed and are replaced by real data sources
    - There may be more than one piece of functionality that's testable; in that case break down this section into sub-sections including all the details above in each sub-section

3. **Format for each story**
    - Add a section for each story. Each story must have the following sub-sections:

    *References*
    - Add a section that references internal and external documentation for this feature. For example, specific sections and sub-sections of the PRD and TDD. Include file paths.
    - Include external documentation if the story involves calling APIs or web services external to the project (look up URLs for documentation ensuring the pages refer to the correct version of the API/service being called)

    *Tasks*
    - A list of HIGHLY detailed tasks, described in enough detail that a junior engineer could complete the task in under an hour without having to look anything up
    - Include a fine level of detail on what needs to be done such as (but not limited to):  Functions to be written, assumed data input to functions, assumed data output of funcitons, UI elements to be added, data that will be applied to each UI element, handlers for each UI element, instructions for wiring up components to existing APIs/components.
    - Provide sample code when helpful especially for interface definitions or data types

    *Tests*
    - Go back to the start of this story's section, and prior to the tasks, add the unit test section.
    - Insert detailed tests that will be written to validate the functionality of each of the tasks. 
    - The output should be a list of highly specific test descriptions (which might be a rather long list for a complex story).
    - Be extremely specific about each test, explaining what it will verify, specifying what function(s) will be called, the inputs, and the expected outputs, and side effects that will be verified (if any).
    - Include tests for both common cases and all edge cases (think through inputs like user input, security concerns, unexpected values, etc)
    - Avoid unit tests for trival things like ensuring variables exist or checking that UI elements exist -- focus instead on ensuring that input data is manipulated and/or displayed correctly.

4. **Add Formatting**
    - Each story, task, and test line-item should start with a markdown empty check box [ ] 
    - At the top, include the creation date, and last updated date/time
