# Claude Code 101 Completion Notes

**Date:** 2026-06-14
**Course:** Anthropic SkillJar - Claude Code 101
**Status:** Completed
**Quiz Score:** 5/5 - 100%
**Quiz Time:** 2 minutes

# Overview

Today I completed the Anthropic SkillJar **Claude Code 101** course as part of the broader objective of extracting reusable agentic software-development concepts for the **Force Agentic Command Lab** repository.

The course focused on how Claude Code operates as an agentic coding system with direct access to files, repositories, terminals, developer tools, and external services.

The primary value of the course was not Claude-specific command memorization, but the engineering principles behind:

- agentic coding loops.
- repository-aware context gathering.
- permission-controlled execution.
- structured development workflows.
- context management.
- persistent project instructions.
- delegated subagents.
- external tool integration.
- deterministic lifecycle automation.

---

# Curriculum Covered

## Claude Code Fundamentals

- What Claude Code is.
- How Claude Code differs from standard Claude chat.
- What an AI agent is.
- The agentic execution loop.
- Tool use and environmental interaction.
- Permission modes.
- Supported interfaces and installation methods.

## Development Workflow

- Writing the first prompt.
- Approval mode.
- Auto-accept mode.
- Plan Mode.
- Explore → Plan → Code → Commit.
- Defining success criteria.
- Test-driven verification.
- Code review with subagents.
- Commit, push, and pull-request automation.

## Context and Memory

- The context window.
- Automatic context compaction.
- `/compact`.
- `/clear`.
- `/context`.
- Persistent project memory with `CLAUDE.md`.
- User-level and project-level memory.
- Context-efficient task delegation.

## Extensibility and Automation

- Subagents.
- Model Context Protocol.
- MCP server scopes.
- HTTP and stdio MCP servers.
- Context costs of external tools.
- Skills versus MCP versus CLI tools.
- Lifecycle hooks.
- Deterministic action blocking.
- Team-shared hook configuration.

---

# Key Concepts Learned

## 1. Claude Code as an Agentic Coding Tool

Claude Code is an agentic software-development system that can directly interact with a development environment.

It can:

- read and understand a repository.
- edit multiple files.
- run terminal commands.
- execute tests.
- inspect command output.
- search external documentation.
- revise its implementation based on observed results.

The central difference between standard chat and Claude Code is environmental access.

```text
Standard AI Chat
=
Prompt
→ Text Response
```

```text
Claude Code
=
Prompt
→ Repository Inspection
→ Tool Action
→ Observation
→ Verification
→ Iteration
```

Claude Code does not merely suggest what the developer should do. It can perform the work inside the repository.

---

# The Agentic Loop

Claude Code follows an iterative loop:

```text
Prompt
→ Gather Context
→ Take Action
→ Verify Results
→ Repeat or Finish
```

If the action does not satisfy the objective, Claude gathers more context and tries again.

The user can intervene during the loop by:

- providing additional context.
- correcting assumptions.
- interrupting execution.
- steering the implementation.
- changing permissions.

Engineering interpretation:

```text
Goal
=
Reference State

Claude Code
=
Controller

Tool Calls
=
Control Actions

Repository and Terminal
=
Environment

Command Outputs and Tests
=
Feedback
```

The agent continues until the observed result satisfies the requested objective or a stopping condition is reached.

---

# Tools

Tools allow Claude Code to interact with its environment.

Examples include:

- file-reading tools.
- file-editing tools.
- repository search.
- shell commands.
- test execution.
- web search.
- browser control.
- external integrations.

The agent determines:

```text
Which tool to use
+
When to use it
+
What inputs to provide
+
How to interpret the result
```

Tools transform the model from a passive text generator into an active software-development agent.

---

# Permissions

Claude Code supports multiple permission modes.

## Approval Mode

Claude asks permission before:

- editing files.
- running shell commands.

```text
Proposed Action
→ Human Approval
→ Execution
```

This provides maximum supervision.

## Auto-Accept Mode

File edits are automatically approved, while shell commands still require permission.

```text
File Edits
=
Automatic

Commands
=
Approval Required
```

This reduces workflow friction while preserving some command-level control.

## Plan Mode

Plan Mode uses read-only tools to investigate the repository and produce an implementation plan before making changes.

```text
Inspect
→ Analyze
→ Ask Questions
→ Propose Plan
→ Human Review
→ Execute
```

Plan Mode is particularly useful for:

- unfamiliar repositories.
- complex features.
- architectural changes.
- large refactors.
- code review.
- high-risk modifications.

---

# Installation and Interfaces

Claude Code can be used through:

- Terminal.
- Visual Studio Code.
- JetBrains IDEs.
- Claude Desktop.
- Claude on the web.

## Terminal

The terminal interface generally receives new features first.

Claude Code is launched from a project directory using:

```bash
claude
```

The directory from which Claude Code is launched defines its accessible workspace.

```text
Launch Directory
+
Subdirectories
=
Available Repository Scope
```

This makes directory selection part of the permission and safety model.

## Visual Studio Code

Claude Code can be installed as an Anthropic extension and opened through the command palette or sidebar.

## JetBrains

Claude Code can be installed through the JetBrains Marketplace and used through an integrated pane.

## Desktop

Claude Desktop provides a graphical Code mode with folder selection, permission settings, and cloud execution options.

## Web

The web version operates primarily on GitHub-hosted repositories and is useful for remote repository work.

---

# Writing Effective Prompts

A strong Claude Code prompt should include:

```text
Goal
+
Scope
+
Constraints
+
Desired Behavior
+
Validation Criteria
```

Example structure:

```text
Implement the requested feature.

Inspect the existing architecture first.

Do not modify unrelated files.

Follow current repository conventions.

Add or update tests.

Run the relevant validation commands.

Report the files changed and any remaining uncertainty.
```

A detailed prompt may consume more tokens initially, but it often reduces total context use by lowering ambiguity, unnecessary exploration, and later correction.

```text
Total Context Cost
=
Initial Prompt
+
Exploration
+
Clarification
+
Correction
```

Clear prompts increase the first term slightly while reducing the remaining terms.

---

# Explore → Plan → Code → Commit

The course's central development workflow is:

```text
Explore
→ Plan
→ Code
→ Commit
```

## Explore

Claude investigates:

- repository structure.
- architecture.
- relevant files.
- existing dependencies.
- related implementations.
- tests.
- project conventions.

The objective is to build an accurate model before changing anything.

## Plan

Claude creates an implementation plan containing:

- files to modify.
- dependencies.
- implementation sequence.
- risks.
- edge cases.
- test strategy.
- acceptance criteria.

This is the cheapest stage for correcting mistakes.

```text
Cost of Correction Before Coding
<
Cost of Correction After Coding
```

## Code

Claude executes the approved plan.

```text
Approved Plan
→ Edit
→ Build
→ Test
→ Inspect
→ Revise
```

The plan acts as a reference trajectory against which implementation progress can be measured.

## Commit

Before committing:

- run tests.
- inspect the Git diff.
- perform independent review.
- resolve accepted findings.
- obtain human approval.

The resulting code can then be committed, pushed, and converted into a pull request.

---

# Success Criteria

Claude needs an explicit definition of what correct behavior looks like.

Weak criterion:

```text
Add image conversion.
```

Stronger criterion:

```text
Convert uploaded PNG and JPEG files to WebP before storage.

Do not recompress existing WebP files.

Preserve current validation and API behavior.

Return controlled errors when conversion fails.

Add tests for successful and failed conversions.

Ensure all existing tests continue to pass.
```

A useful success contract contains:

```text
Expected Behavior
+
Failure Behavior
+
Compatibility Requirements
+
Objective Validation
```

Without explicit criteria, an agent may stop when the implementation appears plausible rather than when it is proven correct.

---

# Testing and Verification

Claude Code can continuously validate changes using:

- unit tests.
- integration tests.
- build commands.
- linters.
- type checkers.
- static analysis.
- browser testing.
- runtime inspection.
- Git diffs.

```text
Edit
→ Test
→ Observe Failure
→ Revise
→ Test Again
```

However:

```text
Passing Tests
≠
Guaranteed Correctness
```

The reliability of agentic verification is bounded by the quality of the tests.

```text
Verification Quality
≤
Test Quality
```

An incomplete or incorrect test suite may produce false confidence.

---

# Code Review

Before pushing a pull request, the course recommends using a fresh code-review subagent.

```text
Implementation Agent
≠
Review Agent
```

The main agent may be biased by:

- its original assumptions.
- its implementation decisions.
- its debugging history.
- attachment to its own solution.

A fresh reviewer receives a separate context and can inspect the result more independently.

The reviewer should be restricted to read-only tools.

Its job is to:

- identify defects.
- detect regressions.
- assess security.
- inspect test quality.
- flag unnecessary complexity.
- report repository convention violations.

A useful review report should include:

```text
Severity
+
File Location
+
Observed Problem
+
Why It Matters
+
Suggested Correction
```

---

# Git and Pull-Request Workflow

Claude Code includes support for automating common Git operations.

## `/commit-push-pr`

This Skill combines:

```text
Commit
→ Push
→ Create Pull Request
```

It reduces repetitive manual steps but should only be used after:

- testing.
- diff review.
- independent review.
- human approval.

## Session Linking

When Claude creates a pull request, the session may be linked to it.

The session can later be resumed with:

```bash
claude --from-pr <PR_NUMBER>
```

This is useful for:

- addressing review comments.
- fixing CI failures.
- updating tests.
- resolving merge conflicts.
- continuing feature development.

A pull request becomes a durable coordination object containing:

```text
Code State
+
Review State
+
CI State
+
Human Feedback
+
Agent Session Reference
```

---

# Context Management

The context window is Claude's finite working memory.

It contains:

- prompts.
- conversation history.
- file contents.
- tool definitions.
- tool calls.
- command outputs.
- search results.
- implementation plans.
- intermediate findings.

```text
Context Usage
=
Messages
+
Files
+
Tools
+
Outputs
+
Session State
```

Because the context window is finite, context must be managed deliberately.

---

# Automatic Compaction

When the context window approaches its limit, Claude Code automatically compacts the session.

Compaction:

- summarizes important decisions.
- removes low-value tool outputs.
- compresses prior conversation.
- frees context for continued work.

```text
Detailed Session
→ Compressed Summary
→ Continued Work
```

Compaction is lossy and may remove:

- small constraints.
- exact error messages.
- rejected alternatives.
- implementation reasoning.
- file-specific details.

Therefore:

```text
Compacted State
≠
Perfect Session Replay
```

---

# Context Commands

## `/compact`

Use `/compact` when continuing the same feature but approaching the context limit.

```text
Long Session
→ /compact
→ Preserve Summary
→ Continue
```

## `/clear`

Use `/clear` when starting a new feature or unrelated task.

```text
Previous Context
→ /clear
→ Fresh Session
```

This prevents old assumptions from biasing new work.

## `/context`

Use `/context` to inspect:

* context usage
* remaining capacity
* major context categories
* tool overhead

```text
/context
→ Observe Memory State
→ Remove Waste
→ Continue Efficiently
```

General rule:

```text
Same Objective
→ /compact

New Objective
→ /clear
```

---

# CLAUDE.md

`CLAUDE.md` provides persistent project memory.

It is automatically read at the start of every session and appended to the prompt context.

```text
Session Context
=
Current Prompt
+
CLAUDE.md
+
Retrieved Repository Information
```

A typical file may include:

```markdown
# Project

This is a Next.js application using the App Router, Tailwind, and Drizzle ORM.

# Commands

- Development: `pnpm dev`
- Tests: `pnpm test`
- Lint: `pnpm lint`

# Code Style

- Use two-space indentation.
- Prefer named exports.
- Use server actions where appropriate.
```

## Project-Level Memory

A project-level `CLAUDE.md` lives in the repository root and should normally be committed.

It contains:

- project stack.
- commands.
- architecture rules.
- coding conventions.
- testing requirements.
- environment notes.

## User-Level Memory

A user-level `CLAUDE.md` applies across projects and contains personal preferences.

## Memory Principle

```text
Temporary Feature State
→ Session Context

Stable Project Knowledge
→ CLAUDE.md
```

Repeated corrections are good candidates for persistent memory.

```text
Repeated Correction
→ Stable Rule
→ CLAUDE.md
```

The file should remain compact because it consumes context at the beginning of every session.

---

# Starting Without CLAUDE.md

The course recommends initially working without a `CLAUDE.md`.

This allows the developer to observe where Claude repeatedly needs correction.

```text
Start Without Memory
→ Observe Repeated Failure
→ Add Stable Rule
```

This keeps the file empirical and focused rather than speculative and bloated.

When enough patterns have emerged, `/init` can generate an initial draft.

```text
/init
→ Generated CLAUDE.md Draft
→ Human Review
```

---

# Subagents

Subagents are delegated agents that operate in isolated context windows.

```text
Main Agent
→ Delegate Task
→ Subagent Investigates
→ Subagent Returns Summary
→ Main Agent Continues
```

Their main benefits are:

- context isolation.
- parallel execution.
- specialization.
- reduced exploration noise.
- independent review.

A subagent may inspect many files and tool outputs while returning only a concise summary to the main agent.

```text
Subagent Context
=
Exploration Journey

Main Context
=
Final Findings
```

---

# Creating Subagents

Subagents are managed through:

```text
/agents
```

They are defined using Markdown files with YAML frontmatter.

A subagent definition may include:

- name.
- description.
- purpose.
- tool permissions.
- invocation conditions.
- operating instructions.
- persistent memory.
- preloaded Skills.

Example conceptual structure:

```markdown
---
name: code-reviewer
description: Reviews code changes for correctness and regressions.
tools:
  - Read
  - Search
---

Review the current changes.

Do not edit files.

Report findings by severity with file references.
```

---

# Subagent Design Principles

A good subagent should have:

```text
Bounded Objective
+
Defined Scope
+
Least-Privilege Tools
+
Clear Output Format
+
Stopping Condition
```

Example:

```text
Locate all authentication endpoints.

Return file paths, request flow, dependencies, and uncertainty.

Do not edit files.
```

Subagents should return:

- conclusions.
- evidence.
- file paths.
- risks.
- uncertainty.
- recommendations.

Independent context reduces implementation bias but does not guarantee correctness.

```text
Fresh Context
≠
Ground Truth
```

---

# Model Context Protocol

Model Context Protocol is an open standard that connects Claude Code to external tools and data sources.

```text
Claude Code
→ MCP Client
→ MCP Server
→ External System
```

External context may exist in:

- databases.
- issue trackers.
- cloud systems.
- productivity applications.
- documentation services.
- messaging platforms.
- public repositories.

Examples:

- Linear MCP for issue details.
- Documentation MCP for current API references.
- Slack MCP for team communication.
- Database MCP for structured queries.

---

# MCP Server Types

## HTTP Servers

HTTP MCP servers are remotely hosted services accessed over a network.

```text
Claude Code
→ Network
→ Hosted MCP Server
```

## Stdio Servers

Stdio MCP servers are local processes communicating through standard input and output.

```text
Claude Code
→ Local Process
→ stdin/stdout
```

A local MCP server is executable software and should therefore be treated as a security-sensitive component.

---

# Adding and Managing MCP Servers

Add a server using:

```bash
claude mcp add
```

Manage active servers using:

```text
/mcp
```

This allows the user to:

- inspect connected servers.
- view available tools.
- check connection status.
- reconnect servers.
- disable unnecessary integrations.

---

# MCP Scope

MCP servers may be configured at three levels.

## Local

Available only in the current project for the current user.

## User

Available across all projects for the current user.

## Project

Stored in:

```text
.mcp.json
```

and committed to version control so the team receives consistent MCP configuration.

```text
.mcp.json
≈
Agent Tooling Manifest
```

Secrets should not be stored in shared configuration.

---

# MCP Context Cost

MCP tool definitions consume context even when not actively used.

```text
Available Work Context
=
Total Context
-
MCP Tool Definitions
-
Other Instructions
```

Too many active servers can:

- reduce usable context.
- increase tool-selection ambiguity.
- increase security exposure.
- make agent behavior less predictable.

The course recommends disabling servers that are not relevant to the current task.

If an external capability has a CLI equivalent, the CLI may be more context-efficient.

Examples:

```text
GitHub MCP
vs.
gh
```

```text
AWS MCP
vs.
aws
```

---

# MCP vs. Skills vs. CLI

```text
MCP
=
External tools and data systems

Skill
=
Reusable procedural knowledge

CLI
=
Installed command-line capability

CLAUDE.md
=
Persistent project guidance
```

Skills usually load their name and description initially, then load full instructions only when needed.

MCP servers may load full tool definitions upfront.

When MCP tools consume more than approximately 10% of the context window, Claude Code may switch to tool-search mode and discover tools on demand.

This saves context but may reduce tool-discovery reliability.

---

# Hooks

Hooks run commands at specific points in Claude Code's lifecycle.

Their defining characteristic is determinism.

```text
Prompt Instruction
→ Model Usually Complies

Hook
→ Command Always Runs When Triggered
```

Hooks are configured through:

```text
/hooks
```

or:

```text
.claude/settings.json
```

---

# Hook Events

## `PreToolUse`

Runs before a tool call.

Used for:

- blocking dangerous commands.
- protecting files.
- enforcing permission rules.
- validating tool input.

## `PostToolUse`

Runs after a tool call.

Used for:

- formatting.
- logging.
- linting.
- validation.
- post-processing.

## `UserPromptSubmit`

Runs after a prompt is submitted but before Claude processes it.

## `Stop`

Runs when Claude finishes responding.

## `Notification`

Runs when Claude sends a notification.

---

# Hook Exit Codes

For `PreToolUse` hooks:

```text
Exit Code 0
→ Allow Action
```

```text
Exit Code 2
→ Block Action
→ Return stderr to Claude as Feedback
```

```text
Other Nonzero Exit Code
→ Report Non-Blocking Error
```

This allows deterministic safety enforcement.

Example:

```text
Proposed rm -rf Command
→ PreToolUse Hook
→ Detect Dangerous Pattern
→ Exit 2
→ Block Command
```

Claude receives the reason and may replan.

---

# Hooks as Deterministic Guardrails

Hooks are useful for requirements that must always be enforced.

Examples:

- Always format edited files.
- Block writes to production configuration.
- Log every executed shell command.
- Prevent direct commits to the main branch.
- Notify the user when a task finishes.

General rule:

```text
Preference
→ Prompt or CLAUDE.md

Invariant
→ Hook, Test, CI, or Access Control
```

Hooks are not complete security boundaries.

Critical safety should also use:

* operating-system permissions
* branch protection
* CI rules
* sandboxing
* secret management
* cloud IAM policies

```text
Defense in Depth
=
Hook
+
Repository Policy
+
Infrastructure Permission
```

---

# Shared Team Configuration

Project hooks can be stored in:

```text
.claude/settings.json
```

and committed to version control.

Hook scripts should use:

```text
CLAUDE_PROJECT_DIR
```

to reference project files reliably regardless of the current working directory.

A useful structure is:

```text
.claude/
└── settings.json

scripts/
└── hooks/
    ├── block-dangerous-command.py
    ├── format-edited-file.sh
    └── log-tool-call.py
```

Hook configuration is executable repository policy and should be code-reviewed.

---

# Force Agentic Command Lab Translation

The course can be summarized as:

```text
Reliable Agentic Coding
=
Repository Context
+
Explicit Planning
+
Tool Access
+
Bounded Permissions
+
Iterative Verification
+
Persistent Memory
+
Context Isolation
+
Deterministic Guardrails
+
Human Accountability
```

---

# Claude-Specific Concept Translation

```text
Claude Code
→ Agentic coding environment
```

```text
CLAUDE.md
→ Persistent repository instruction file
```

```text
Plan Mode
→ Read-only analysis and implementation planning
```

```text
Subagents
→ Isolated specialist workers
```

```text
MCP
→ External tool and data integration protocol
```

```text
Skills
→ Reusable procedural instruction packages
```

```text
Hooks
→ Deterministic lifecycle automation
```

```text
/compact
→ Lossy session-state compression
```

```text
/clear
→ Active context reset
```

```text
/commit-push-pr
→ Automated delivery workflow
```

---

# Relationship to the Markovian Unit-Step Protocol

The Claude Code agentic loop closely matches the **Markovian Unit-Step Protocol**:

```text
Observe
→ Act
→ Verify
→ Update State
→ Repeat
```

Claude Code provides the broad agentic execution architecture.

MUSP adds stricter transition control:

```text
One Action
→ One Observation
→ One Verified State Transition
```

The relationship can be summarized as:

```text
Explore → Plan → Code → Commit
=
Project-Level State Machine
```

```text
MUSP
=
Step-Level Transition Policy
```

Hooks can enforce transition guards:

```text
State
→ Proposed Action
→ PreToolUse Validation
→ Execution
→ PostToolUse Verification
→ New State
```

Subagents act as isolated observation and analysis modules:

```text
Main State
+
Subagent Summaries
→ Updated Main State
```

`CLAUDE.md` improves the initial state estimate:

```text
Initial State
=
Persistent Project Knowledge
+
Current Prompt
+
Retrieved Evidence
```

---

# Personal Additions

## 1. Permission-Aware Unit-Step Protocol

For high-risk tasks, the Claude Code workflow can be combined with MUSP:

```text
Inspect One State
→ Propose One Action
→ Human Approval
→ Execute
→ Verify Output
→ Continue
```

This is useful for:

- destructive shell operations.
- ROS environment changes.
- dependency upgrades.
- production configuration.
- large repository refactors.
- hardware deployment.

## 2. Context as State Estimation

Context should be treated as an estimated internal state rather than permanent truth.

```text
Agent State Estimate
=
Persistent Memory
+
Session History
+
Retrieved Evidence
+
Tool Results
```

Compaction creates a compressed estimate:

```text
ŝₜ = Compress(s₀, s₁, ..., sₜ)
```

A reliable agentic workflow should preserve critical state externally through:

- code.
- tests.
- commits.
- project instructions.
- plans.
- issue trackers.
- documentation.

## 3. Deterministic vs. Probabilistic Control

The course establishes a useful control hierarchy:

```text
Prompt
→ Probabilistic Task Guidance

CLAUDE.md
→ Persistent Semantic Guidance

Hook
→ Deterministic Lifecycle Control

Test / CI
→ Outcome Verification

Infrastructure Permission
→ Hard Security Boundary
```

The correct mechanism should be selected according to how strongly the requirement must be enforced.

---

# Overall Assessment

## Strengths

- Strong introduction to agentic coding concepts.
- Clear explanation of the agentic execution loop.
- Practical Explore → Plan → Code → Commit workflow.
- Useful treatment of context as a finite engineering resource.
- Strong emphasis on planning before implementation.
- Good distinction between persistent memory and session context.
- Practical introduction to subagents and context isolation.
- Useful explanation of MCP, Skills, and CLI trade-offs.
- Hooks provide a strong bridge between AI instructions and deterministic automation.
- Encourages independent review and human verification.

## Weaknesses

- Primarily focused on Claude-specific tooling and commands.
- Some workflow automation may create false confidence if tests or review criteria are weak.
- Subagent independence reduces session bias but does not create true model diversity.
- MCP integrations can significantly increase context and security overhead.
- Persistent memory and hooks require maintenance to avoid stale or harmful rules.
- The course introduces concepts broadly but does not deeply cover secure sandboxing, prompt injection, or formal verification.

---

# Quiz Results

The final assessment contained five questions.

Results:

```text
Correct Answers: 5 / 5
Score: 100%
Elapsed Time: 2 minutes
```

Topics tested:

- Definition of an AI agent.
- Automatic context compaction.
- Explore → Plan → Code → Commit workflow.
- Automatic loading of `CLAUDE.md`.
- Claude Code fundamentals.

---

# Final Takeaway

Claude Code is best understood not as an advanced autocomplete tool, but as a repository-aware agentic execution environment.

Its effectiveness depends on:

```text
Good Context
+
Clear Goals
+
Explicit Plans
+
Relevant Tools
+
Reliable Tests
+
Controlled Permissions
+
Independent Review
+
Deterministic Guardrails
+
Human Judgment
```

The central operating principle is:

```text
Explore before planning.

Plan before coding.

Verify before committing.

Use persistent memory for stable knowledge.

Use subagents for isolated work.

Use MCP only for necessary external capabilities.

Use hooks when behavior must be guaranteed.

Keep the human responsible for the final state.
```

---