---
name: GudaSpec: Research
description: Transform user requirements into constraint sets through parallel exploration and analysis
category: GudaSpec
tags: [gudaspec, research, constraints, exploration, subagents]
---

<!-- GUDASPEC:START -->
**Core Philosophy**
- If the project is detected to lack `./openspec/` dir, prompt the user to initialize the project using `/gudaspec:init`.
- Research produces **constraint sets**, not information dumps. Each constraint narrows the solution space.
- Constraints tell subsequent stages "don't consider this direction," enabling mechanical execution without decisions.
- The output is "約束集合 + 可验证的成功判据" (constraint sets + verifiable success criteria).
- Eliminate ambiguity through structured exploration and user interaction.
- Strictly adhere to **OpenSpec** rules when writing **standardized spec-structured projects**.

**Guardrails**
- **NEVER** divide subagent tasks by roles (e.g., "架构师agent", "安全专家agent"). 
- **ALWAYS** divide by context boundaries (e.g., "user-related code", "authentication logic", "infrastructure config").
- Each subagent context must be self-contained with independent output.
- **MANDATORY**: Use `mcp__auggie-mcp__codebase-retrieval` to minimize search/grep/find operations.
- All subagents must follow the same structured output template for aggregation.
- Do not make architectural decisions—surface constraints that guide decisions.

**Steps**
0. **Initialize OpenSpec Exploration**
   - Run `/opsx:explore <user question>` always first.

1. **Initial Codebase Assessment**
   - Combine user requirements with quick codebase scan using `mcp__auggie-mcp__codebase-retrieval`.
   - Determine project scale: single directory vs. multi-directory structure.
   - **Decision point**: If code spans multiple subdirectories or modules → enable parallel Explore subagents.
   - Document finding: "Single agent serial exploration is inefficient; parallel subagents required."

2. **Define Exploration Boundaries (Context-Based Division)**
   - Identify natural context boundaries in the codebase (NOT functional roles).
   - Example divisions (adapt to project type):
     * Subagent 1: Core domain logic (business rules, state management, data models)
     * Subagent 2: External interfaces (network protocols, API handlers, I/O operations)
     * Subagent 3: Infrastructure & configuration (build scripts, configs, deployment)
   - Each boundary should be self-contained: no cross-communication needed between subagents.
   - Define exploration scope and expected output for each subagent.
   - **Note**: Subagent execution is blocking; wait for all results before proceeding to aggregation.

3. **Prepare Standardized Output Template**
   - Define a unified JSON schema that all Explore subagents must follow:
   ```json
   {
     "module_name": "string - context boundary explored",
     "existing_structures": ["list of key structures/patterns found"],
     "existing_conventions": ["list of conventions/standards in use"],
     "constraints_discovered": ["list of hard constraints that limit solution space"],
     "open_questions": ["list of ambiguities requiring user input"],
     "dependencies": ["list of dependencies on other modules/systems"],
     "risks": ["list of potential risks or blockers"],
     "success_criteria_hints": ["observable behaviors that indicate success"]
   }
   ```
   - Communicate template to all subagents for consistency.

4. **Parallel Subagent Dispatch**
   - For each defined context boundary, spawn an Explore subagent with:
     * **Explicit instruction**: "You MUST use `mcp__auggie-mcp__codebase-retrieval` to reduce search operations."
     * Defined scope and context boundary.
     * Required output template (from Step 3).
     * Clear success criteria: complete analysis of assigned boundary.
   - Monitor subagent execution and collect structured reports.

5. **Aggregate and Synthesize Reports**
   - Collect all subagent JSON outputs.
   - Merge findings into unified constraint sets:
     * **Hard constraints**: Technical limitations, existing patterns that cannot be violated.
     * **Soft constraints**: Conventions, preferences, style guides.
     * **Dependencies**: Cross-module relationships that affect implementation order.
     * **Risks**: Potential blockers that need mitigation.
   - Identify **open questions** from all reports that require user clarification.
   - Synthesize **success criteria** from scenario hints across all contexts.

6. **Multi-Model Constraint Validation (Conditional)**
   - **Trigger conditions** (enable when ANY applies):
     * Aggregated constraints have high uncertainty or conflicting signals
     * Cross-module dependencies are complex (>3 modules involved)
     * User requirements touch security, concurrency, or distributed systems
     * Subagent reports contain unresolved contradictions
   - When triggered, invoke both MCP tools for cross-validation:
     * Use `mcp__gemini__gemini`: "Review these aggregated constraints for system-level blind spots. Identify architectural constraints that single-module exploration may have missed. Focus on: cross-cutting concerns, implicit coupling, scalability boundaries. Output format: [BLIND_SPOT] <description> → [CONSTRAINT_TO_ADD] <specification>."
     * Use `mcp__codex__codex`: "Analyze these constraints for implementation-level gaps. Identify low-level technical constraints that high-level exploration overlooked. Focus on: API contracts, error propagation paths, resource lifecycle. Output format: [GAP] <description> → [CONSTRAINT_TO_ADD] <specification>."
   - Merge model outputs into constraint sets; resolve conflicts by escalating to user.
   - **Skip this step** if exploration was straightforward and constraints are clear.

7. **User Interaction for Ambiguity Resolution**
   - Compile prioritized list of open questions from aggregated reports (including multi-model validation findings).
   - Use `AskUserQuestions` tool to present questions systematically:
     * Group related questions together.
     * Provide context for each question.
     * Suggest default answers when applicable.
   - Capture user responses as additional constraints.
   - Update constraint sets with confirmed decisions.

8. **Generate OpenSpec Proposal**
   - Run `/opsx:ff <requirement-description>` and follow it.
   - When generate `proposal.md`, strictly prohibit abbreviating user requirements; only expansion is allowed. The generated document MUST NOT contain any ambiguous content. For example, specific project files mentioned by the user must be accurately referenced down to the file path, and should not be abbreviated in a way that misleads document readers into thinking it refers to an online-searched project rather than a local one.

**Reference**
- Inspect codebase structure: `ls -R` or `mcp__auggie-mcp__codebase-retrieval`
- Validate subagent outputs conform to template before aggregation.
- Use `AskUserQuestions` for ANY ambiguity—do not assume or guess.
- Always base judgments on project codes, strictly prohibiting the use of general knowledge for speculation. It is permissible to indicate uncertainty to users.
<!-- GUDASPEC:END -->
