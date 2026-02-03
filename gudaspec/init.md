---
name: GudaSpec: Init
description: Initialize OpenSpec environment with MCP tools validation for the current project.
category: GudaSpec
tags: [openspec, init, setup, mcp]
---
<!-- GUDASPEC:START -->
**Guardrails**
- Detect the current operating system (Linux, macOS, Windows) and adjust shell commands accordingly.
- All example commands below assume a Linux/Unix environment; adapt syntax for PowerShell on Windows if detected.
- Do not proceed to the next step until the current step completes successfully.
- Provide clear, actionable error messages when a step fails.
- Respect user's existing configurations and avoid overwriting without confirmation.

**Steps**
1. **Detect Operating System**
   - Identify the current OS using appropriate methods (`uname -s` on Unix-like systems, or environment variables on Windows).
   - Inform the user which OS was detected and note any command adaptations that will be made.

2. **Check and Install OpenSpec**
   - Verify if `openspec` CLI is installed by running `openspec --version` or `which openspec` (Linux/macOS) / `where openspec` (Windows).
   - If not installed, install globally using:
     ```bash
     npm install -g @fission-ai/openspec@latest
     ```
   - On Windows, use the same npm command in PowerShell or Command Prompt.
   - Confirm installation success by running `openspec --version` after installation.

3. **Initialize OpenSpec for Current Project**
   - Run the initialization command:
     ```bash
     openspec init --tools claude
     ```
   - Verify that `openspec/` directory structure is created successfully.
   - Report any initialization errors and suggest remediation steps.

4. **Configure Multi-Language Support**
   - Ask the user which language they prefer for generated artifacts (default: English).
   - If non-English is selected, edit `openspec/config.yaml` to add language context:
     ```yaml
     schema: spec-driven

     context: |
       语言：中文（简体）
       所有产出物必须用简体中文撰写。
       技术术语（如 API、REST、GraphQL）保留英文。
       代码示例和文件路径保持英文。
     ```
   - Supported language examples:
     - **Chinese (Simplified)**: `语言：中文（简体）\n所有产出物必须用简体中文撰写。`
     - **Japanese**: `言語：日本語\nすべての成果物は日本語で作成してください。`
     - **Spanish**: `Idioma: Español\nTodos los artefactos deben escribirse en español.`
     - **Portuguese (Brazil)**: `Language: Portuguese (pt-BR)\nAll artifacts must be written in Brazilian Portuguese.`
   - If `config.yaml` already has a `context` field, append the language instruction to it.
   - Verify the configuration by running:
     ```bash
     openspec instructions proposal --change test-change
     ```
   - The output should include the language context.

5. **Validate MCP Tools Availability**
   - Check if `mcp__codex__codex` tool is available and responsive.
   - Check if `mcp__gemini__gemini` tool is available and responsive.
   - Check if `mcp__augment-context-engine__codebase-retrieval` tool is available and responsive.
   - For each unavailable tool, display a clear warning message with installation instructions:
     - **Codex MCP**: "The `mcp__codex__codex` tool is not available. Please install it from: https://github.com/GuDaStudio/codexmcp"
     - **Gemini MCP**: "The `mcp__gemini__gemini` tool is not available. Please install it from: https://github.com/GuDaStudio/geminimcp"
     - **Augment Context Engine**: "The `mcp__augment-context-engine__codebase-retrieval` tool is not available. This tool is MANDATORY for Research phase. Please install it from: https://www.augmentcode.com/"
   - Explain that these MCP tools will be required for subsequent GudaSpec workflows.

6. **Summary Report**
   - Display a summary of the initialization status:
     - OpenSpec installation: ✓/✗
     - Project initialization: ✓/✗
     - Multi-language configuration: ✓/✗ (language name)
     - `mcp__codex__codex` availability: ✓/✗
     - `mcp__gemini__gemini` availability: ✓/✗
     - `mcp__augment-context-engine__codebase-retrieval` availability: ✓/✗
   - If any components are missing, list the required actions before proceeding with other GudaSpec commands.

**Reference**
- OpenSpec CLI documentation: Run `openspec --help` for available commands.
- OpenSpec Multi-Language Guide: https://github.com/Fission-AI/OpenSpec/blob/main/docs/multi-language.md
- OpenSpec Customization Guide: https://github.com/Fission-AI/OpenSpec/blob/main/docs/customization.md
- Codex MCP installation: https://github.com/GuDaStudio/codexmcp
- Gemini MCP installation: https://github.com/GuDaStudio/geminimcp
- Augment Context Engine: https://www.augmentcode.com/
- For Node.js/npm issues, ensure Node.js >= 18.x is installed.
- On permission errors during global npm install, consider using `sudo` (Linux/macOS) or running terminal as Administrator (Windows), or configure npm to use a user-writable directory.
<!-- GUDASPEC:END -->
