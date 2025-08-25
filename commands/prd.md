---
allowed-tools: Bash(ls:*), Bash(mkdir:*), Bash(date:*), Powershell
description: Create or update a PRD based on an existing doc, a description, or even just an idea.
---
# PRD
Create or update a PRD based on an existing doc, a description, or even just an idea.

## Usage
```
/prd <product/feature name + description>
```

## PRD name
1. **Confirm PRD file name**
   - Check `docs\` for an existing folder that matches this project, and an existing `prd.md` inside that folder
   - If a PRD seems to already exists, warn the user and ask user whether they want to update the existing PRD, write a new one, or cancel
   - If the folder doesn't exist, come up with an appropriate namme (Must contain only lowercase letters, numbers, and hyphens, Must start with a letter, No spaces or special characters allowed)
   - If th PRD file doesn't exist, create `prd.md` within that folder

## If thin requirements
- Ask for product requirements until the basic needs are understood enough
- Prompt the user to provide product details, including user needs and requirments (allow the user to reference documents as input).
- Keep prompting the user with questions until there's enough detail to work from (basic user needs will suffice)

## Check for related documents
- Check `docs` for other product documents, PRDs, design documents, or descriptions that are relevant to this PRD.

## Write the PRD
**After collecting requirements**, explicitly **invoke the `product-manager` agent** to udpate or draft the PRD, providing all the known requirement details and supporting docs, as well as the file path. Provide these instructions:
- At the top include the title (e.g. "My Feature PRD")
- At the top, also include the creation date, and last updated date/time
- At the bottom include a Document History template for summarizing future edits to the document, and include a line for creation of the doc

1. **Ensure you understand user's unmet needs fully**
   - Read any related documents, overall project vision/design documents, related PRDs, etc (in the docs folder)
   - Ensure you understand who the user of the feature is and any relevant attributes (examples: their role, skill level, envioronment, work flow)
   - Ask the user for clarification, especially if the user or their needs are ambiguous

2. **Answer "what needs to be true?"**
   - Understand "what needs to be true" for the user needs to be satisfied
   - This is not specific to implementation... it's details such that if the product succeeds... what the outcomes will be in terms of the user's experience and what they will get
   - Also this includes user's expectations: how much effort is required on behalf of the user? how often will the user use this feature or be impacted by it? 
   - What about this feature would cuase the user to consider it "easy to use" and "easy to understand"?

2. **Framing the problem and user needs with an abstract**
   - As an introduction to the document, write an abstract almost as if "selling" the feature to the user
   - Describe the bigger picture that the product/feature fits into 
   - Describe what the product/feature will do for the user in terms of benefits, especially solving their unmet needs
   - Explain what will be true for the user when they use it in terms of what they will be able to do, how their life will be better, and how it will be a good experience for them
   - Include a bullet list of benefits if that helps
   - Avoid specific features or implementation details in this stage

3. **Outline major components**
   - Write an index listing all the top-level solutions that will become part of the product to fulfill the goals
   - List them in chronlogical order of implementation (intending for a minimal-usable product as soon as possible, and layering in functionality, so that in an agile workflow, a product champion would be able to test each feature that goes in and provide feedback)
   - Aim for simple and elegant solutions that fully meet each user need without embellishing or increasing scope (MVP mentality)
   - Prefer solutions that maintain degrees of freedom  in case it's necessary to expand the solution later to fully satisfy the user

4. **Write requirements for each component**
   - Apply these instructions to each chapter of the doc which fleshes out the requirements for an item in the outline, documenting requirements for each feature or component that needs to be built
   - Translate the requirements into user acceptance criteria: What should a user be able to do or experience once the requirements are met? How will the engineers and designers know that the feature is "done" (in terms of functionality, user ease, performance, etc).
   - This is where you define a product vision that fulfills the user's needs and anything missing will not be built -- so state assumptions, be specific, include edge cases, and include a section on how to handle unexpected issues and errors
   - Include a brief user journey that describes the "golden path" (common case) for how the feature will impact the user and how the user will interact with it. This section should help engineers and designers understand the desired workflow so they can visualize how the requirements come together
   - Identify trade-offs and state reasons for the preferred solution over obvious alternatives
   - Avoid implementation details (leave that to engineers) focusing instead on user needs and requirements
   
## Style

Do's:
    - For each requirement, write a human-readable and brief blurb about what the need is and why
    - Defined a list of acceptance criteria
    - Group the acceptance criteria in important topics such as "Core need", "Usability expectations", "Discovery expectations", "Convenience expectations", "Performance expecations", "Visual design expecations", "Security expectations", and "Failure expectations". Adjust the topics as needed to be relevant to each feature (only include topics that make sense; add additional topics not listed here if they are relevant).,  
    - Write a "golden path" user journey to avoid ambuguity about how the feature would be used

Don'ts:
    - When writing requirements, make them readable by avoiding repeating the same words on every line, such as "As a user...".
    
Example:

    [1] Feature: Email address change
        
        Users need a way to change their email address. Motivation: If a user changes their primary email account or entered the wrong account during signup, they would want to avoid missing important emails from us which would go to their unmonitored email account.

        **Acceptance criteria**:
         *Core need*:
         - Users have UI area to edit the email address they want to use for communication from us
         - Hitting "enter" or pressing a button submits their new email address
         
         *Usability expectations*:
         - Users can only input valid email addresses (following length and format requirements defined in email RFCs)
         - It's easy to tell if the email entered is valid or not
         - Invalid emails can't be submitted and the reason is explained to the user
         - To prevent the user accidentally submiting an incorrect email, users are prompted to confirm and shown the new email address in the confirmation
         - Icons or shorthand must have tooltips describing what they do
         - Icons must be industry-standard for their purpose; avoid ambiguous icons preferring text instead

         *Convenience expectations*
         - Fewer than 2 clicks to change the email
         - One of the first Account options in settings

         *Discovery expectationns*:
         - The UI element is in the "Account" section of "Settings"
         - The FAQ pages in the support area describe this feature and provide a deep-link to the appropriate sub-area of settings

         *Visual design expectations*:
         - The font size, layout, and spacing compared must match similar controls in the Settings area
         - The user will be notified of success or failure via the common snackbar

         *Performance expectations*
         - The performance of typing new text and pressing the button should seem "instantaneous" (less than 20ms)

         *Security expectations*
         - The user must have entered their password within the last week (if not, the user is prompted to re-authenticate)

         *Failure expectations*
         - On failure, the user will be notified via the snackbar of what went wrong
         - If the error is potentially resolve-able by the user, tell them how they might resolve it, like "A network error occurred - check your connection"

        **User journey**:
        - Starting from the home page
        - Click their profile button
        - Click settings
        - Enter new email; button is grayed out until the email is a valid one, with gentle text explaining that the email must be valid before submitting
        - Email entered in full, submit
        - Prompted for 
        - Confirmation dialog shows correct address, confirm
        - Snackbar says successful with a green emoji letting me know it went OK

