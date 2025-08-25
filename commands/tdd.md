---
allowed-tools: Bash(ls:*), Bash(mkdir:*), Bash(date:*), Powershell
description: Create or update a PRD based on an existing doc, a description, or even just an idea.
---
# TDD
Create or update a TDD (technical design document) based on an existing PRD or design document.

## Usage
```
/tdd <feature folder>
```

## Validate input
1. **Confirm folder name and PRD (or similar design document)**
   - The folder specified should be under `docs/` and contain a PRD. Check for similar names. If the folder is not found, or there is no PRD/design doc in the foler, reply an error saying that the command must be called with an existing project folder for this feature

2. **Confirm that the input documents are valid**
   - Read the file(s) provided describe user needs and requirements
   - If the files do not seem to be PRDs or do not provide clear user requirements, explain what the shortcomings are as a bulletted list (be specific, citing the doc) and ask the user to kindly fill in the required details.

## TDD file name
1. **Confirm TDD file name**
   - The file should be within a folder named for the feature like `docs/<feature>/prd.md`; if the `docs/<feature>` folder doesn't exist yet, come up with an appropriate namme (Must contain only lowercase letters, numbers, and hyphens, Must start with a letter, No spaces or special characters allowed) and create the folder.
   - If a file called `tdd.md` already exists within the feature's folder, warn the user and ask user whether they want to update the existing TDD, write a new one, or cancel
   - If a new document needs to be written come up with an appropriate file namme (Must contain only lowercase letters, numbers, and hyphens, Must start with a letter, No spaces or special characters allowed)
   - The final file name should be `docs/<feature slug>/tdd.md`

## Write the TDD
Explicitly **invoke the `feature-architect` agent** to udpate or draft the TDD provide these instructions:

Throughout the writing of the TDD, read relevant sections of code to understand context for what needs to be built and how the new features will integrate with code that already exists.

0. **Read the supporting documents**
    - Read the entire PRD in detail

1. **Write an introduction restating requirements**
    - Describe what the document is about and the high-level user need(s) being addressed
    - Explain what the user needs and requirements imply about the TECHINCAL requirements at a high level. (E.g. "Based on these requirements, we need an architecture that will...")

2. **Outline major work areas**
    - Outline all the major areas of work involved in building the system (e.g. a list of the Epics), with brief descriptions of each
    - Come up with the outline so that each epic can be run and verified by a human being. This means that the first epic should be the smallest possible "runnable" code, and each epic after that should add something to the existing code that's "runnable".

3. **Write an abstract of architecture and technical requirements**
    - Outline how the system will be architected
    - Explain how subsystems and features will interact
    - Describe code abstractions that will be created for clean architecture
    - Explain major technology selections, data structure/storage, and validation methods
    - Explain how the system will be architected for testing and verificaiton
    - Choose which technologies will be used for unit tests and integration tests (must be existing automated test suites)

4. **Write out the work items in heirarchical fashion**
    - Produce Epics and Stories for all the work
    - Each Story should be 1 story point in length
    - Each Epic should be at most 7 story points (approximately 1 dev-week of work)
    - At the start of each epic, explain how the epic builds on previous work, including prior epics and existing code in the project
    - At the end of each epic, write acceptance criteria explaining how the "product champion" will be able to verify that the new features were implemented correctly

5. **Sequencing of epics to favor working code**
   - Sequence epics so that the code is "testable" by a product champion and builds upon the work of prior epics.
   - Start with core piece of "testable" functionality in the first sprint. 
   - Take a greedy "building on success" strategy where each subsequent epics adds to the funcitonality of the previous ones (avoiding the strategy of creating lots of independent building blocks that don't fit together until the end).
   - The goal is that the product champion to run the code and perform acceptance testing at every stage.

6. **Iterate on the architecture**
   - Treat the first draft of the TDD as a "discovery pass" to identify all the work involved
   - Then, perform a rewrite where you take into account the totality of the work and make edits/adjustments to refine the architecture and work items

7. **Add Formatting**
    - At the top include the title (e.g. "My Feature TDD")
    - At the top, also include the creation date, and last updated date/time
    - At the bottom include a Document History template for summarizing future edits to the document, and include a line for creation of the doc


    