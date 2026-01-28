---
name: GudaSpec: Implementation
description: Execute approved OpenSpec changes via multi-model collaboration with Codex/Gemini.
category: GudaSpec
tags: [openspec, implementation, multi-model, codex, gemini]
---
<!-- GUDASPEC:START -->
**Guardrails**
- If the project is detected to lack `./openspec/` dir, prompt the user to initialize the project using `/gudaspec:init`.
- Never apply external model prototypes directly—all Codex/Gemini outputs serve as reference only and must be rewritten into readable, maintainable, production-grade code.
- Keep changes tightly scoped to the requested outcome; enforce side-effect review before applying any modification.
- Minimize documentation—avoid unnecessary comments; prefer self-explanatory code.

**Steps**
1. Run `openspec view` to inspect current project status and review `Active Changes`; Use `AskUserQuestions` tool ask the user to confirm which proposal ID they want to implement and wait for explicit confirmation before proceeding.
2. Run `/opsx:apply <proposal_id>` and then follow it.
3. When performing a specific coding task, refer to the code prototype provided by an effective model based on **task characteristics** (NOT technology stack):
   - **Route A: Gemini Kernel** — `mcp__gemini__gemini` tool for visual analysis, global understanding, and technical research tasks. Trigger when: screenshot analysis, large-scale code comprehension (>10 files), architecture dependency analysis, framework comparison, state flow mapping. Output is directional reference only.
   - **Route B: Codex Kernel** — `mcp__codex__codex` tool for precise implementation, deep debugging, and optimization tasks. Trigger when: production-grade code writing, bug localization/fixing, performance optimization, network protocol implementation, framework module extension. Context must be <50k tokens with entry file and line numbers provided.
   - **Route C: Collaborative Decision** — for complex tasks requiring both global understanding and precise implementation. Flow: Gemini outputs architecture → Claude reviews and supplements → Codex implements → Cross-validation.
   - **Mandatory constraint**: When communicating with Codex/Gemini, the prompt **must explicitly require** returning a `Unified Diff Patch` only; external models are strictly forbidden from making any real file modifications.
4. Upon receiving the diff patch from Codex/Gemini, **NEVER apply it directly**; rewrite the prototype by removing redundancy, ensuring clear naming and simple structure, aligning with project style, and eliminating unnecessary comments.
5. Before applying any change, perform a mandatory side-effect review: verify the change does not exceed task scope, does not affect unrelated modules, and does not introduce new dependencies or break existing interfaces; make targeted corrections if issues are found.
6. For each completed task, conduct multi-model reviews using Codex`mcp__codex__codex` / Gemini`mcp__gemini__gemini`, requiring iterative reviews until receiving dual-model **LGTM approval**.
7. MUST follow the `/opsx:apply <proposal_id>`.
<!-- GUDASPEC:END -->
