# CLAUDE.md

### Intent
- **LIMIT CHANGES TO REQUESTED SCOPE** - Do not modify code or behavior outside the scope of what's been requested
- **BACKGROUND** - Check for documents under docs\ that might be relevant background to what you're doing

### Code Quality
- **NO CODE DUPLICATION** - Check existing codebase to reuse functions
- **NO DEAD CODE** - Either use or delete completely
- **CONSISTENT NAMING** - Follow existing codebase patterns
- **SIMPLICITY** - Simple, easy-to-read functions with comments instead of fancy/complex design patterns
- **NO RESOURCE LEAKS** - Close connections, clear timeouts, remove listeners
- **PROPER THREAD SYNCHRONIZATION** - Do not use sleeps to work around timing issues; fix root issues and use proper thread synchronization patterns
- **COMMON LOCATION FOR INTERFACE FILES** - The interfaces for a single component/library must be kept in a single location (e.g. headers, interfaces, type definitions); avoid creating multiple namespaces for one component unless absolutely necessary

### Maintainability
- **MAINTANABILITY GOAL** - The overarching purpose of all comments is to increase maintainability by ensuring that when future maintainers make changes, they'll have full visibility into the intentionality behind code, as well as how it impacts other parts of the system.
- **FILE HEADER** - At the top of each source file, document what the enclosed code is for. Refer to documentation files (PRDs, TDDs). List other files with a dependency on that file, and other files that take a dependency on the file, to the best of your ability.  
- **CLASS HEADER** - Explain what a class is used for and key architectural decisions
- **FUNCTION COMMENTS** - Document the expectations of the data format of any inputs, and the data format of the outputs. Explain what the function is used for, and how it manipulates the data. Explain any side-effects (including UI).  
- **IF MISSING** - If comments are missing from a file you're about to edit, add comments as you go.

### Error Handling
- **Fail fast** for critical configuration
- **Log and continue** for optional features
- **Graceful degradation** when external services unavailable
- **User-friendly messages** through resilience layer
- **Single error handler** all errors pass through a unified error handler/reporter but only once (subsequent try/catch blocks must not report the same root error)

## AGENT COORDINATION

### When to Use Sub-Agents
- **File Analysis**: Summarizing logs, configs, verbose outputs  with `file-analyzer`
- **UX Question**: Run `ux-design-optmizer` to resolve a design question
- **Requirements clarification**: Run `product-manager` to clarify user and product requirements
- **Architecture clarification**: Run `feature-architect` to clarify functional requirements or resolve architecture questions