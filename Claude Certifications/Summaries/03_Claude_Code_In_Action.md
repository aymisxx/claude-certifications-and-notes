# Claude Code in Action Completion Notes

**Date:** 2026-06-14
**Course:** Anthropic SkillJar - Claude Code in Action
**Status:** Completed
**Quiz Score:** 8/8 - 100%
**Quiz Time:** 4 minutes

# Overview

Today I completed the Anthropic SkillJar **Claude Code in Action** course as part of the broader objective of extracting reusable agentic software-development concepts for the **Force Agentic Command Lab** repository.

The course moved beyond the introductory Claude Code workflow and focused on practical implementation details behind modern coding assistants, including tool use, persistent project memory, context control, custom commands, browser automation, GitHub Actions integration, deterministic hooks, and the Claude Agent SDK.

The primary value of the course was not Claude-specific command memorization, but the engineering principles behind:

- tool-using coding agents.
- structured environment interaction.
- persistent and scoped project memory.
- conversation steering and context control.
- reusable command automation.
- visual browser-based verification.
- repository automation through GitHub Actions.
- deterministic tool-call interception.
- compiler-driven feedback loops.
- programmatic agent orchestration through an SDK.

---

# Curriculum Covered

## Coding Assistant Architecture

- How coding assistants work.
- Gather context → formulate a plan → take action.
- Why language models require tools.
- Structured tool calls.
- Tool execution and result feedback.
- Claude's tool-use strengths.
- Security implications of selective repository access.

## Local Setup and Project Context

- Claude Code installation.
- Local project setup.
- `/init`.
- `CLAUDE.md`.
- `CLAUDE.local.md`.
- `~/.claude/CLAUDE.md`.
- `/memory`.
- `@` file references.
- Importing `AGENTS.md`.
- Screenshot-based communication.

## Planning and Reasoning Control

- Plan Mode.
- `/plan`.
- `Shift + Tab`.
- Editing plans with `Ctrl + G`.
- Effort levels through `/effort`.
- Single-turn `ultrathink`.
- Breadth versus depth in agent reasoning.

## Conversation Control

- Interrupting with Escape.
- Rewinding with double Escape or `/rewind`.
- `/compact`.
- `/clear`.
- `/resume`.
- Persisting repeated corrections.

## Reusable Workflow Automation

- Project-level custom commands.
- `.claude/commands/`.
- Markdown command files.
- `$ARGUMENTS`.
- Automated audit and test workflows.

## External Tool Integration

- Model Context Protocol.
- Playwright MCP.
- Browser automation.
- MCP permissions.
- Visual verification loops.
- Other MCP server categories.

## GitHub Integration

- `/install-github-app`.
- Claude in GitHub Actions.
- `@claude` issue and pull-request mentions.
- Automatic pull-request review.
- Project setup in CI.
- Custom instructions.
- MCP configuration in workflows.
- Explicit `allowed_tools`.

## Hooks

- PreToolUse hooks.
- PostToolUse hooks.
- Hook scopes.
- Hook payloads.
- Exit-code behavior.
- Blocking sensitive files.
- Absolute path recommendations.
- Type-checking hooks.
- AI-based review hooks.
- Additional lifecycle events.
- Hook payload discovery with logging.

## Claude Agent SDK

- `@anthropic-ai/claude-agent-sdk`.
- Programmatic agent queries.
- Streaming JSON events.
- Restricting tools with `allowedTools`.
- Hooks, MCP, subagents, and session resumption through the SDK.

---

# Key Concepts Learned

## 1. Coding Assistants as Tool-Using Agents

A coding assistant is not merely a model that generates source code.

It operates through a development loop:

```text
Gather Context
→ Formulate a Plan
→ Take Action
→ Observe Results
→ Continue or Finish
```

The assistant may need to:

- inspect repository files.
- locate relevant symbols.
- read documentation.
- edit source code.
- execute commands.
- run tests.
- inspect failures.
- revise its implementation.

The model itself cannot directly perform these actions.

```text
Language Model
=
Text Input
→ Text Output
```

```text
Coding Assistant
=
Language Model
+
Tool Definitions
+
Tool Executor
+
Environment
+
Feedback Loop
```

---

# Tool Use

Language models need tools because they cannot directly interact with external systems.

A simplified file-reading workflow is:

```text
User asks about main.go
→ Assistant provides tool instructions
→ Model requests ReadFile(main.go)
→ Host application reads the file
→ File contents return to the model
→ Model answers
```

The model produces a structured request containing:

```text
Tool Name
+
Tool Arguments
```

The surrounding application:

1. parses the request.
2. checks permissions.
3. executes the tool.
4. captures the result.
5. sends the result back to the model.

```text
Model
→ Structured Tool Request
→ Orchestrator
→ External Operation
→ Structured Result
→ Model
```

---

# Why Tool-Use Quality Matters

A model must decide:

- whether a tool is needed.
- which tool is appropriate.
- what arguments to provide.
- in what order to call tools.
- how to interpret failures.
- when enough evidence has been collected.
- when to stop.

Weak tool use:

```text
Read Wrong File
→ Assume Architecture
→ Edit Unrelated Code
→ Skip Verification
```

Strong tool use:

```text
Search Relevant Symbols
→ Read Minimal Files
→ Trace Data Flow
→ Modify Correct Location
→ Run Targeted Validation
```

Tool-enabled performance depends on both:

```text
Tool Capability
×
Model Tool-Selection Skill
```

---

# Extensible Tooling

Claude can often use unfamiliar tools when given clear:

- names.
- descriptions.
- input schemas.
- output structures.

```text
New Tool Definition
→ Model Interprets Capability
→ Model Constructs Tool Call
→ Tool Becomes Part of Workflow
```

This enables extension through:

- MCP servers.
- custom scripts.
- browser tools.
- internal APIs.
- repository-specific utilities.
- cloud integrations.

---

# Security and Selective Retrieval

Claude Code can navigate repositories without necessarily indexing the entire codebase upfront.

```text
Task
→ Search Relevant Areas
→ Read Necessary Files
```

This may reduce unnecessary code exposure compared with workflows that upload or index the entire repository.

However:

```text
Selective Retrieval
→ Potentially Reduced Exposure
```

does not imply:

```text
Tool Use
→ Automatic Security
```

Security still depends on:

- execution environment.
- provider configuration.
- tool implementation.
- network access.
- permissions.
- retention policies.

---

# Installation and Project Setup

Claude Code can be installed through platform-specific commands.

After installation:

```bash
claude
```

launches the CLI.

The first launch includes:

- terminal theme selection.
- account authentication.
- provider configuration when applicable.

The course project used a UI-generation application with:

```bash
npm run setup
```

for dependency and SQLite initialization, followed by:

```bash
npm run dev
```

for local execution.

An optional Anthropic API key may be stored in:

```text
.env
```

The key should never be committed.

```text
API Key
→ Local Environment File
→ Excluded from Version Control
```

---

# Context Quality

A large codebase may contain hundreds of files, but the agent rarely needs all of them at once.

```text
More Context
≠
Better Context
```

A better objective is:

```text
Context Utility
=
Relevant Information
────────────────────
Total Information Loaded
```

Irrelevant context can:

- consume tokens.
- introduce conflicting patterns.
- reduce focus.
- increase search complexity.
- degrade implementation quality.

---

# The `/init` Command

Running:

```text
/init
```

in a new repository asks Claude to analyze:

- project purpose.
- architecture.
- critical files.
- commands.
- dependencies.
- coding patterns.
- repository structure.

Claude then creates a project summary in:

```text
CLAUDE.md
```

The process is:

```text
Repository
→ /init
→ Project Analysis
→ CLAUDE.md Draft
```

The generated file should be reviewed manually.

```text
/init Output
=
Useful Starting Point

Not:
Authoritative Project Specification
```

---

# CLAUDE.md Memory Hierarchy

Claude Code recognizes three common project-memory locations.

## Project-Level Memory

```text
CLAUDE.md
```

Purpose:

- shared with the team.
- committed to source control.
- contains repository-wide guidance.
- usually generated initially with `/init`.

## Personal Project Memory

```text
CLAUDE.local.md
```

Purpose:

- applies only to the current user and project.
- contains personal project-specific preferences.
- is not committed.

## Global User Memory

```text
~/.claude/CLAUDE.md
```

Purpose:

- applies across all projects on the machine.
- contains global personal preferences.

The effective memory stack is:

```text
Global User Instructions
+
Personal Project Instructions
+
Shared Project Instructions
+
Current Prompt
```

---

# Updating Memory

The current command for memory management is:

```text
/memory
```

The older `#` shortcut shown in previous course videos has been removed.

Repeated stable corrections should be persisted.

Example:

```text
Use comments sparingly.
Only comment code when the intent is not obvious.
```

```text
Repeated Stable Correction
→ Persistent Memory
```

Memory changes apply from the next request onward.

---

# File Mentions with `@`

The `@` syntax allows explicit file injection.

Example:

```text
How does the auth system work? @auth
```

Claude can present matching files and attach the selected content.

```text
Prompt
+
@File
→
Prompt with Explicit File Context
```

Benefits:

- less repository exploration.
- lower ambiguity.
- more predictable answers.
- clearer evidence.
- lower context waste.

---

# Referencing Files from CLAUDE.md

Files can also be referenced persistently.

Example:

```markdown
The database schema is defined in @prisma/schema.prisma.
Reference it when reasoning about stored data.
```

This is useful for compact, central files such as:

- schemas.
- architecture summaries.
- stable interfaces.
- policy documents.

It should not be used indiscriminately because imported file contents consume context repeatedly.

---

# Importing AGENTS.md

A repository that already contains:

```text
AGENTS.md
```

does not need duplicated instructions.

A Claude-specific adapter can begin with:

```markdown
@AGENTS.md
```

followed by Claude-specific rules.

```text
AGENTS.md
=
Cross-Agent Repository Instructions

CLAUDE.md
=
Claude-Specific Extension Layer
```

---

# Screenshots as Context

Screenshots provide precise visual evidence for UI changes.

The course uses:

```text
Ctrl + V
```

to paste screenshots into Claude Code.

Screenshots help communicate:

- layout defects.
- spacing.
- visual hierarchy.
- component location.
- color issues.
- responsive behavior.

The resulting loop is:

```text
Rendered State
→ Screenshot
→ Claude Inspection
→ Code Search
→ Modification
→ Browser Verification
```

---

# Plan Mode

Plan Mode can be activated with:

```text
/plan
```

or by cycling modes using:

```text
Shift + Tab
```

In Plan Mode, Claude:

- reads more files.
- investigates architecture.
- creates an implementation plan.
- shows the proposed steps.
- waits for approval.

```text
Repository Exploration
→ Detailed Plan
→ Human Review
→ Execution
```

Plan Mode is best for breadth-heavy work:

- multi-file features.
- architecture changes.
- cross-component modifications.
- large refactors.
- repository-wide impact analysis.

---

# Editing Plans

Pressing:

```text
Ctrl + G
```

opens the plan in a text editor.

The user can directly modify the execution specification before approval.

```text
Claude Draft Plan
→ Human Editing
→ Final Approved Plan
→ Agent Execution
```

This creates a stronger planning contract than conversational correction alone.

---

# Effort Levels

Claude's reasoning depth can be controlled through:

```text
/effort
```

Higher effort generally means:

- more reasoning.
- more tokens.
- more latency.
- potentially stronger performance on difficult problems.

Lower effort generally means:

- faster execution.
- lower cost.
- suitability for routine tasks.

For a single turn, the course uses:

```text
ultrathink
```

as a signal for deeper reasoning.

```text
/effort
→ Session-Level Reasoning Setting

ultrathink
→ Single-Turn Reasoning Signal
```

---

# Plan Mode vs. Thinking Depth

Plan Mode and reasoning effort address different dimensions.

```text
Plan Mode
=
Breadth
```

It is useful when the agent must understand many files or subsystems.

```text
Higher Effort
=
Depth
```

It is useful for:

- difficult debugging.
- algorithms.
- concurrency.
- mathematical reasoning.
- complex state transitions.

Some tasks require both:

```text
High Breadth
+
High Depth
→
Plan Mode + Higher Effort
```

---

# Conversation Control

Claude Code provides several mechanisms for steering a session.

## Escape

Pressing:

```text
Escape
```

interrupts Claude immediately.

Use it when Claude:

- heads in the wrong direction.
- expands scope.
- attempts too many tasks.
- starts making unnecessary changes.

```text
Wrong Current Action
→ Escape
→ Redirect
```

## `/rewind`

Use:

```text
/rewind
```

or press Escape twice to return to an earlier user message.

```text
Useful Early Context
+
Distracting Later Branch
→ Rewind
→ Continue from Earlier State
```

## `/compact`

Use when continuing the same long task.

```text
Long Related Session
→ /compact
→ Compressed Useful State
→ Continue
```

## `/clear`

Use when starting an unrelated task.

```text
Old Task Context
→ /clear
→ Fresh Context
```

## `/resume`

Use to reopen a previous conversation.

```text
Previous Session
→ /resume
→ Restored Work
```

General rule:

```text
Wrong Now
→ Escape

Wrong Since Earlier
→ Rewind

Same Mission, Too Much History
→ Compact

Different Mission
→ Clear
```

---

# Custom Commands

Claude Code supports custom project commands stored in:

```text
.claude/
└── commands/
```

A file such as:

```text
audit.md
```

creates:

```text
/audit
```

The command file contains reusable Markdown instructions.

```text
Custom Command
=
Named Reusable Prompt Procedure
```

---

# Command Arguments

Custom commands can accept runtime input through:

```text
$ARGUMENTS
```

Example:

```markdown
Write comprehensive tests for: $ARGUMENTS

Testing conventions:

- Use Vitest with React Testing Library.
- Place tests in a colocated `__tests__` directory.
- Name files `[filename].test.ts(x)`.
- Use `@/` imports.
```

Invocation:

```text
/write_tests the use-auth.ts file
```

```text
Command Template
+
Runtime Argument
→
Customized Workflow
```

---

# Commands vs. Skills vs. Hooks

```text
Custom Command
=
User-Invoked Reusable Workflow
```

```text
Skill
=
Automatically Selected Reusable Expertise
```

```text
Hook
=
Deterministic Lifecycle Reaction
```

```text
CLAUDE.md
=
Persistent Project Guidance
```

Selection rule:

```text
Need User Invocation
→ Custom Command

Need Automatic Semantic Selection
→ Skill

Need Guaranteed Execution
→ Hook
```

---

# Playwright MCP

The Playwright MCP server can be added with:

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

It gives Claude browser-control capabilities such as:

- opening pages.
- navigating.
- clicking.
- typing.
- inspecting page state.
- evaluating rendered output.
- testing interaction flows.

This creates a closed-loop UI workflow:

```text
Write UI Code
→ Open Browser
→ Observe Rendered Output
→ Interact
→ Detect Defect
→ Modify Code
→ Retest
```

---

# MCP Permissions

Local MCP permissions may be pre-approved in:

```text
.claude/settings.local.json
```

Example:

```json
{
  "permissions": {
    "allow": ["mcp__playwright"],
    "deny": []
  }
}
```

The double underscore is significant:

```text
mcp__playwright
```

Pre-approval reduces repeated prompts but expands the autonomous action surface.

```text
Fewer Approval Interruptions
↔
Greater Agent Freedom
```

---

# Visual Prompt Optimization

The course demonstrates a self-improving UI-generation loop:

```text
Generate Component
→ Render in Browser
→ Inspect Styling
→ Modify Generation Prompt
→ Generate Again
→ Compare
```

This allows the agent to improve prompt files based on actual visual output rather than source code alone.

```text
Code Inspection
+
Rendered-State Inspection
→ Better UI Decisions
```

---

# GitHub Integration

Claude Code provides an official GitHub integration configured through:

```text
/install-github-app
```

The setup process:

1. installs the GitHub application.
2. configures authentication.
3. creates a pull request with workflow files.
4. adds GitHub Actions under `.github/workflows/`.

---

# Mention Workflow

After setup, users can mention:

```text
@claude
```

inside an issue or pull request.

Claude may:

```text
Read Request
→ Analyze Repository
→ Build Plan
→ Execute Task
→ Reply in GitHub
```

---

# Automatic Pull-Request Review

The integration can automatically review new pull requests.

Claude may:

- inspect the diff.
- analyze impact.
- identify risks.
- inspect tests.
- post a report.

```text
New Pull Request
→ Automated Claude Review
→ Findings Posted
```

This is an additional review layer, not a substitute for human review.

---

# GitHub Actions Setup

The execution environment may require project setup steps.

Example:

```yaml
- name: Project Setup
  run: |
    npm run setup
    npm run dev:daemon
```

Custom instructions may tell Claude:

- what has already been configured.
- where logs are stored.
- which services are running.
- what tools are available.
- what assumptions are valid.

---

# MCP in GitHub Actions

MCP servers can be configured inside the workflow.

Example:

```yaml
mcp_config: |
  {
    "mcpServers": {
      "playwright": {
        "command": "npx",
        "args": [
          "@playwright/mcp@latest",
          "--allowed-origins",
          "localhost:3000;cdn.tailwindcss.com;esm.sh"
        ]
      }
    }
  }
```

Origin allowlists reduce browser-navigation scope.

```text
Browser Automation
+
Origin Allowlist
→ Reduced External Reach
```

---

# Explicit Tool Permissions in CI

GitHub Actions cannot ask interactively for permission.

Every tool must be explicitly listed.

Example:

```yaml
allowed_tools: "Bash(npm:*),Bash(sqlite3:*),mcp__playwright__browser_snapshot,mcp__playwright__browser_click"
```

```text
CI Agent
=
Predeclared Capability Set
```

The correct principle is:

```text
Allowed Tools
=
Only Tools Required by the Workflow
```

---

# Hooks

Hooks intercept Claude Code lifecycle events and run commands deterministically.

Normal flow:

```text
Model Requests Tool
→ Tool Executes
→ Result Returns
```

With hooks:

```text
Model Requests Tool
→ PreToolUse Hook
→ Tool Executes
→ PostToolUse Hook
→ Result and Feedback Return
```

---

# Hook Configuration Scopes

## Global

```text
~/.claude/settings.json
```

Applies across projects.

## Shared Project

```text
.claude/settings.json
```

May be committed and shared.

## Personal Project

```text
.claude/settings.local.json
```

Applies only to the current user and project.

---

# PreToolUse Hooks

A PreToolUse hook runs before a tool call.

It may:

- allow the action.
- block the action.
- send feedback to Claude.

Example use cases:

- block sensitive file access.
- prevent dangerous shell commands.
- protect production configuration.
- enforce branch policy.

---

# PostToolUse Hooks

A PostToolUse hook runs after the tool call.

It cannot undo the original action, but it may:

- format files.
- run tests.
- run a type checker.
- run a linter.
- log changes.
- send additional feedback.

```text
Edit Completed
→ PostToolUse Validation
→ Feedback
→ Claude Corrects Result
```

---

# Hook Payloads

Hook commands receive JSON through standard input.

Example:

```json
{
  "session_id": "2d6a1e4d-6...",
  "transcript_path": "/Users/sg/...",
  "hook_event_name": "PreToolUse",
  "tool_name": "Read",
  "tool_input": {
    "file_path": "/code/queries/.env"
  }
}
```

The hook must:

```text
Read stdin
→ Parse JSON
→ Inspect Event and Input
→ Decide
→ Exit Correctly
```

---

# Hook Exit Codes

```text
Exit Code 0
→ Allow or Continue
```

```text
Exit Code 2
→ Block PreToolUse Action
→ Send stderr to Claude
```

Other nonzero exit codes generally report errors but are not equivalent to a hard block.

---

# Blocking Sensitive Files

A hook can block the Read tool from opening `.env`.

However:

```text
Read
→ {"file_path": "..."}
```

```text
Grep
→ {"pattern": "...", "path": "..."}
```

```text
Bash
→ {"command": "..."}
```

Each tool has a different input shape.

A Read-only check does not prevent:

```bash
cat .env
```

Therefore:

```text
Sensitive File Protection
=
Permission Deny Rules
+
Tool-Specific Hooks
+
Filesystem Permissions
```

---

# Absolute Paths

Hook scripts should use absolute paths to reduce:

- path interception.
- binary planting.
- working-directory ambiguity.
- accidental execution of the wrong script.

A shareable pattern is:

```text
settings.example.json
+
$PWD Placeholder
+
Setup Script
→ settings.local.json
```

This creates machine-specific absolute paths from a shared template.

---

# Type-Checking Hooks

A PostToolUse hook may run:

```bash
tsc --noEmit
```

after file edits.

This catches cases where Claude changes a function signature but misses call sites.

```text
File Edit
→ Type Checker
→ Cross-File Error
→ Feedback to Claude
→ Repair
```

The compiler acts as a deterministic observer.

---

# AI Review Hooks

A more advanced hook may launch a separate Claude instance to review changes.

Example:

```text
Edit in queries/
→ Reviewer Agent
→ Search for Existing Similar Queries
→ Report Duplication
→ Main Agent Reuses Existing Logic
```

Benefits:

- fresh context.
- architecture consistency.
- reduced duplication.
- immediate semantic review.

Costs:

- additional latency.
- additional API usage.
- similar model blind spots.
- higher configuration complexity.

Therefore, expensive review hooks should monitor only high-value directories.

---

# Additional Hook Events

The course also introduced:

- `Notification`.
- `Stop`.
- `SubagentStop`.
- `PreCompact`.
- `UserPromptSubmit`.
- `SessionStart`.
- `SessionEnd`.

Each event has a different JSON payload.

Hook logic must not assume that every event contains:

- `tool_name`.
- `tool_input`.
- `tool_response`.

---

# Discovering Hook Payloads

A temporary diagnostic hook can log actual input.

Example:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "jq . > post-log.json"
          }
        ]
      }
    ]
  }
}
```

```text
Unknown Payload Schema
→ Log stdin
→ Trigger Event
→ Inspect JSON
→ Implement Correct Parser
```

This follows the principle:

```text
Observe First
→ Implement Second
```

---

# Claude Agent SDK

The Claude Agent SDK runs the Claude Code agent loop programmatically.

The correct TypeScript package is:

```text
@anthropic-ai/claude-agent-sdk
```

The similarly named:

```text
@anthropic-ai/claude-code
```

is the CLI package and is not intended for import.

---

# SDK Installation

```bash
mkdir sdk-demo
cd sdk-demo
npm init -y
npm install @anthropic-ai/claude-agent-sdk
```

---

# Minimal SDK Example

```javascript
import { query } from "@anthropic-ai/claude-agent-sdk";

const prompt = "List the files in the current directory";

for await (const message of query({ prompt })) {
  console.log(JSON.stringify(message, null, 2));
}
```

Run with:

```bash
node index.mjs
```

The SDK returns a stream of JSON events including:

- text.
- tool calls.
- tool results.
- intermediate events.
- completion information.

---

# Streaming Agent Events

The SDK uses asynchronous iteration because an agentic task unfolds as a sequence.

```text
Prompt
→ Tool Call
→ Tool Result
→ More Reasoning
→ More Tool Calls
→ Final Response
```

```text
Agent Task
≠
One API Response
```

Instead:

```text
Agent Task
=
Sequence of State-Transition Events
```

---

# Restricting SDK Tools

The tool set can be narrowed with:

```javascript
for await (
  const message of query({
    prompt,
    options: {
      allowedTools: ["Read", "Glob"],
    },
  })
) {
  // Process events.
}
```

This corresponds to the CLI's:

```text
--allowedTools
```

```text
Available Action Space
=
Allowed Tool Set
```

---

# SDK Capabilities

The SDK supports:

- custom system prompts.
- MCP servers.
- hooks.
- subagents.
- session resumption.
- streaming event handling.
- tool restrictions.

This enables custom systems such as:

- automated code reviewers.
- repository onboarding tools.
- CI failure investigators.
- documentation synchronizers.
- security auditors.
- domain-specific coding agents.

---

# SDK vs. CLI

```text
Claude Code CLI
=
Human-Operated Agent Interface
```

```text
Claude Agent SDK
=
Application-Operated Agent Runtime
```

The CLI is suitable for direct development.

The SDK is suitable when the agent must become part of:

- an application.
- an automated workflow.
- a CI pipeline.
- a multi-agent system.
- an internal developer platform.

---

# Force Agentic Command Lab Translation

The course can be summarized as:

```text
Practical Agentic Development
=
Tool Use
+
Scoped Context
+
Persistent Memory
+
Conversation Control
+
Reusable Commands
+
External Observability
+
CI Automation
+
Deterministic Hooks
+
Programmatic Orchestration
+
Human Accountability
```

---

# Claude-Specific Concept Translation

```text
Tool Use
→ Structured environment interaction
```

```text
/init
→ Repository onboarding and memory initialization
```

```text
CLAUDE.md
→ Shared project memory
```

```text
CLAUDE.local.md
→ Personal project memory
```

```text
~/.claude/CLAUDE.md
→ Global user memory
```

```text
@file
→ Explicit context injection
```

```text
Plan Mode
→ Breadth-oriented repository planning
```

```text
/effort
→ Reasoning-depth control
```

```text
Escape
→ Immediate trajectory interruption
```

```text
/rewind
→ Conversational checkpoint restoration
```

```text
Custom Commands
→ Named reusable prompt procedures
```

```text
Playwright MCP
→ Browser-based observability and action
```

```text
GitHub Actions Integration
→ Event-driven repository automation
```

```text
PreToolUse Hook
→ Deterministic action guard
```

```text
PostToolUse Hook
→ Deterministic post-action observer
```

```text
Claude Agent SDK
→ Programmable agent runtime
```

---

# Relationship to the Markovian Unit-Step Protocol

The course aligns strongly with the **Markovian Unit-Step Protocol**:

```text
Observe
→ Act
→ Verify
→ Update State
→ Repeat
```

Tool use provides actions:

```text
aₜ = π(sₜ)
```

Tool results provide observations:

```text
oₜ₊₁ = Environment(aₜ)
```

The working state updates as:

```text
sₜ₊₁ = Update(sₜ, oₜ₊₁)
```

Plan Mode constructs a state trajectory:

```text
s₀
→ s₁
→ s₂
→ ...
→ s_goal
```

Escape blocks an undesirable transition:

```text
Wrong Proposed Action
→ Interrupt
→ No State Transition
```

Rewind restores an earlier conversational state:

```text
Bad Branch
→ Rewind
→ Earlier State
→ New Branch
```

PreToolUse hooks guard transitions:

```text
Proposed Action
→ Policy Check
→ Allow or Block
```

PostToolUse hooks inspect the resulting state:

```text
Executed Action
→ New State
→ Validation
→ Feedback
```

The SDK exposes these transitions as a programmatic event stream.

---

# Personal Additions

## 1. Breadth-Depth-Execution Separation

The course supports a useful three-part control model:

```text
Plan Mode
→ Breadth
```

```text
Effort
→ Depth
```

```text
MUSP
→ Execution Discipline
```

A difficult repository task may therefore use:

```text
Plan Broadly
→ Reason Deeply
→ Execute One Verified Step at a Time
```

## 2. Deterministic and Probabilistic Layers

```text
Prompt
→ Probabilistic Task Guidance
```

```text
CLAUDE.md
→ Persistent Semantic Guidance
```

```text
Custom Command
→ Reusable User-Invoked Procedure
```

```text
Hook
→ Deterministic Lifecycle Control
```

```text
Compiler / Test
→ Objective Validation
```

```text
OS / Git / IAM
→ Hard External Boundary
```

## 3. Agent Observability

Reliable agentic systems need visibility into:

- tool calls.
- tool results.
- hook payloads.
- context state.
- generated diffs.
- test output.
- browser behavior.
- CI behavior.
- SDK event streams.

```text
No Observability
→ No Reliable Verification
```

## 4. Agent Permissions as an Action-Space Constraint

The tool allowlist defines what the agent can physically do.

```text
Agent Action Space
=
Allowed Tools
×
Workspace Scope
×
External Permissions
```

Reducing the action space improves safety and predictability.

---

# Overall Assessment

## Strengths

- Strong practical explanation of tool use.
- Clear distinction between language models and coding-assistant runtimes.
- Useful treatment of memory hierarchy.
- Excellent conversation-control techniques.
- Practical custom-command workflow.
- Strong browser-automation example using Playwright MCP.
- Clear GitHub Actions integration path.
- Detailed and useful hook implementation guidance.
- Good emphasis on deterministic enforcement.
- Valuable compiler-feedback and AI-review hook examples.
- Useful introduction to the Agent SDK.
- Strong compatibility with controlled multi-agent workflows.

## Weaknesses

- Several examples are tightly coupled to Claude-specific commands and configuration.
- Some course videos contain outdated shortcuts or package names.
- AI-based review hooks may be expensive and still share model-level blind spots.
- Hooks can create supply-chain and local-execution risks.
- GitHub automation can become dangerous if tool permissions are too broad.
- MCP integrations add context, security, and maintenance overhead.
- Visible reasoning and higher effort can create false confidence if outputs are not independently verified.
- The course does not deeply formalize sandboxing, prompt injection resistance, or production-grade policy enforcement.

---

# Quiz Results

The final assessment contained eight questions.

Results:

```text
Correct Answers: 8 / 8
Score: 100%
Elapsed Time: 4 minutes
```

Topics tested:

- the fundamental limitation of language models.
- explicit MCP tool permissions in GitHub Actions.
- Plan Mode versus reasoning depth.
- the three CLAUDE.md memory scopes.
- `$ARGUMENTS` in custom commands.
- PreToolUse blocking behavior.
- sensitive-file protection with Read and Grep.
- the primary purpose of hooks.

---

# Final Takeaway

Claude Code in Action shows that a capable coding assistant is not simply a model with programming knowledge.

It is a layered engineering system:

```text
Language Model
+
Tool Protocol
+
Repository Context
+
Persistent Memory
+
Planning Controls
+
Conversation Controls
+
Reusable Procedures
+
External Integrations
+
Deterministic Hooks
+
Programmatic SDK
+
Human Oversight
```

The central operating principle is:

```text
Give the agent only the context it needs.

Interrupt bad trajectories early.

Persist stable corrections.

Encode repeated workflows.

Use browser tools to observe real outputs.

Use hooks when behavior must be guaranteed.

Restrict tools according to least privilege.

Use the SDK when the agent must become part of a larger system.

Keep humans responsible for the final state.
```

---