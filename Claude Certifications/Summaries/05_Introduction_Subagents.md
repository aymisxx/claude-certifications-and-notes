# Introduction to Subagents Completion Notes

**Date:** 2026-06-15
**Course:** Anthropic SkillJar - Introduction to Subagents
**Status:** Completed

# Overview

Today I completed the Anthropic SkillJar **Introduction to Subagents** course as part of the broader effort to extract reusable multi-agent engineering concepts for the **Force Agentic Command Lab** repository.

The course focused on one of the most important ideas in modern agent systems: **delegation through isolated workers**. Rather than forcing every task through a single context window, subagents allow work to be distributed into specialized execution contexts that independently explore, analyze, and report results.

The primary value of the course was not Claude-specific configuration, but the broader engineering principles behind:

- context isolation.
- delegated execution.
- task decomposition.
- worker specialization.
- capability design.
- information compression.
- output schemas.
- action-space restriction.
- multi-agent coordination.
- effective delegation patterns.

---

# Curriculum Covered

## What Are Subagents

- Isolated execution contexts.
- Parent agent delegation.
- Context preservation.
- Summary-based communication.
- Built-in subagents.
- Custom subagents.

## Creating Subagents

- Agent creation workflow.
- User vs project scope.
- Tool permissions.
- Model selection.
- Agent configuration files.
- System prompts.
- Automatic delegation.

## Designing Effective Subagents

- Description engineering.
- Input prompt shaping.
- Output schemas.
- Termination conditions.
- Obstacle reporting.
- Tool minimization.
- Capability design.

## Using Subagents Effectively

- Research workflows.
- Code review workflows.
- Custom prompt specialization.
- Information compression.
- Delegation tradeoffs.
- Anti-patterns.
- Decision framework.

---

# Key Concepts Learned

## 1. Subagents as Specialized Workers

A subagent is not merely a prompt.

A subagent represents:

```text
Worker
+
Role
+
Context
+
Capabilities
```

Unlike skills, which extend expertise, subagents extend execution.

```text
Skill
→ Knowledge Extension

Subagent
→ Execution Extension
```

---

## 2. Context Isolation

The central idea behind subagents is context isolation.

Without subagents:

```text
Question
→ Searches
→ File Reads
→ Tool Calls
→ Answer
```

Everything accumulates in the main context window.

With subagents:

```text
Question
→ Subagent
→ Exploration
→ Summary
→ Answer
```

Only the summary returns.

The intermediate work is discarded.

This preserves context and enables longer working sessions.

---

## 3. Parent-Agent Architecture

Subagents receive two primary inputs:

```text
System Prompt
+
Task Description
```

The parent agent generates the task and launches the worker.

Execution becomes:

```text
Parent Agent
→ Delegate Task
→ Subagent Executes
→ Summary Returned
→ Parent Continues
```

This resembles hierarchical control systems and multi-agent architectures.

---

## 4. Built-In Subagents

Claude Code provides several built-in workers.

### General Purpose

```text
Exploration
+
Action
```

### Explore

```text
Search
Navigation
Discovery
```

### Plan

```text
Research
Analysis
Planning
```

These provide immediate delegation capabilities without requiring custom configuration.

---

## 5. Subagent Configuration

Subagents are stored as markdown files inside:

```text
.claude/agents/
```

Example:

```text
.claude/agents/
└── code-quality-reviewer.md
```

A typical configuration contains:

```yaml
---
name:
description:
tools:
model:
color:
---
```

followed by the system prompt.

A subagent therefore consists of:

```text
Configuration
+
System Prompt
```

---

## 6. Scope

Subagents may be created at two levels.

### User-Level

```text
Available Across Projects
```

Suitable for personal workflows and reusable workers.

### Project-Level

```text
Available Only Within Repository
```

Suitable for team-specific workflows and project standards.

---

## 7. Tool Access and Capability Design

Subagents should receive only the tools required for their role.

Examples:

### Reviewer

```text
Read
Glob
Grep
Bash
```

### Research Agent

```text
Read
Glob
Grep
```

### Refactoring Agent

```text
Read
Edit
Write
Bash
```

The design principle is:

```text
Capability
=
Task Requirements
```

not

```text
Capability
=
Everything
```

---

## 8. Model Assignment

Subagents can use different Claude models.

### Haiku

```text
Fast Lightweight Worker
```

### Sonnet

```text
Balanced General Worker
```

### Opus

```text
Deep Analysis Worker
```

### Inherit

```text
Use Parent Model
```

This allows different workers to be optimized for different workloads.

---

## 9. Description Engineering

Descriptions serve two purposes.

### Delegation Trigger

The parent agent decides whether the subagent should run.

### Task-Shaping Guidance

The parent agent uses the description to help construct the task prompt.

A strong description improves:

```text
Routing
+
Delegation Quality
+
Task Precision
```

---

## 10. Automatic Delegation

Descriptions can encourage proactive delegation.

Example:

```text
Proactively suggest running this agent...
```

This increases the likelihood that Claude automatically delegates work when appropriate.

---

# Designing Effective Subagents

## Output Formats

One of the strongest lessons in the course is the importance of structured output.

Example:

```text
Summary

Critical Issues

Major Issues

Minor Issues

Recommendations

Approval Status
```

Structured outputs create natural termination conditions.

The subagent knows it is finished when all sections are complete.

---

## Termination Conditions

Without a defined output format:

```text
Research
→ More Research
→ More Research
→ More Research
```

The agent struggles to determine when enough work has been done.

With an output schema:

```text
Fill Required Sections
→ Terminate
```

Output formats therefore function as convergence criteria.

---

## Obstacle Reporting

Subagents should report:

- setup issues.
- workarounds.
- dependency problems.
- configuration quirks.
- special command requirements.

Example:

```text
Obstacles Encountered
```

This prevents the main thread from rediscovering the same issues repeatedly.

---

## Minimal Tool Access

Effective subagents receive only the capabilities required for their task.

Benefits:

- safety.
- predictability.
- specialization.
- reduced unintended behavior.

---

# Using Subagents Effectively

## Research

Research is one of the best subagent use cases.

Example:

```text
Find where JWT validation occurs.
```

The subagent may inspect dozens of files while the parent receives only the final answer.

---

## Code Reviews

Code reviews benefit from context isolation.

The reviewer sees the code independently rather than inheriting all prior design decisions and implementation history.

This creates a more objective review process.

---

## Custom System Prompts

Subagents become particularly valuable when they operate under a different objective than the parent agent.

Examples:

### Copywriting Agent

Optimized for:

```text
Tone
Voice
Audience
```

### Styling Agent

Optimized for:

```text
Design Systems
Spacing Rules
Color Tokens
```

This allows specialized behavior that differs from the default coding-oriented prompt.

---

# Anti-Patterns

## Expert Personas

Examples:

```text
Python Expert
Kubernetes Expert
Docker Guru
```

These rarely add value because Claude already possesses the underlying knowledge.

The worker gains no meaningful new capability.

---

## Sequential Pipelines

Example:

```text
Reproduce Bug
→ Debug Bug
→ Fix Bug
```

This appears elegant but often performs poorly because information is lost between agent handoffs.

Each summary compresses the previous state.

Critical details can disappear.

---

## Test Runner Subagents

Testing frequently requires:

```text
Logs
Errors
Stack Traces
Detailed Output
```

When these details are compressed into summaries, debugging becomes more difficult.

Testing generally performs better directly in the main thread.

---

# Decision Framework

The course ultimately provides a simple decision rule:

```text
Does The Intermediate Work Matter?
```

If:

```text
No
```

Delegate.

If:

```text
Yes
```

Keep the task in the main thread.

---

## Good Subagent Use Cases

```text
Research

Exploration

Code Reviews

Architecture Investigation

Custom Prompt Tasks
```

---

## Poor Subagent Use Cases

```text
Debugging Pipelines

Test Execution

Expert Personas

Tightly Coupled Sequential Workflows
```

---

# Force Agentic Command Lab Translation

The course can be summarized as:

```text
Reusable Agent Workers
=
Role Definition
+
Capability Constraints
+
Context Isolation
+
Task Delegation
+
Structured Reporting
```

Potential examples include:

```text
Repository_Explorer

Paper_Reviewer

Architecture_Auditor

Documentation_Reviewer

Literature_Searcher
```

Each performs exploration independently and reports concise results.

---

# Relationship to the Markovian Unit-Step Protocol

The course aligns strongly with hierarchical variants of the Markovian Unit-Step Protocol.

Main thread:

```text
Observe
→ Delegate
→ Receive Summary
→ Update State
```

Subagent:

```text
Observe
→ Act
→ Verify
→ Report
→ Terminate
```

The subagent effectively executes a temporary state trajectory and returns compressed information to the parent.

---

# Personal Additions

## 1. Subagents as Autonomous Workers

A useful robotics analogy is:

```text
Skill
→ Reusable Controller

Subagent
→ Specialized Robot
```

The worker receives a task, performs independent execution, and reports results.

---

## 2. Information Compression

Subagents are fundamentally information compressors.

```text
Exploration
→ Compression
→ Summary
```

This is both their greatest strength and their greatest limitation.

---

## 3. Multi-Agent Architecture

The course implicitly defines a layered worker architecture:

```text
Parent Agent
→ Delegation

Subagent
→ Execution

Summary
→ Communication
```

This strongly resembles hierarchical multi-robot systems.

---

## 4. Delegate Outcomes, Not Feedback Loops

One of the strongest lessons in the course is:

```text
Delegate Outcomes.

Keep Feedback Loops Local.
```

Research and exploration can be delegated.

Debugging and testing generally should not.

---

# Overall Assessment

## Strengths

- Strong explanation of context isolation.
- Practical delegation guidance.
- Useful agent-design principles.
- Good treatment of tool restrictions.
- Excellent discussion of output formats.
- Valuable anti-pattern analysis.
- Strong connection to real-world workflows.
- Clear decision framework.

## Weaknesses

- Focuses heavily on Claude-specific implementations.
- Limited discussion of inter-agent communication.
- Does not deeply explore agent coordination.
- Memory persistence between agents is not examined.
- Advanced orchestration patterns are only lightly discussed.

---

# Final Takeaway

Introduction to Subagents demonstrates that modern agent systems are not simply larger prompts.

They are collections of specialized workers operating in isolated contexts.

A subagent is not valuable because it claims expertise.

A subagent is valuable because it performs work independently and returns only the information that matters.

The central operating principle is:

```text
Does the intermediate work matter?
```

If the answer is no:

```text
Delegate.
```

If the answer is yes:

```text
Keep the task in the main thread.
```

More broadly:

```text
Task
→ Delegation

Delegation
→ Exploration

Exploration
→ Summary

Summary
→ Action
```

The course reinforces a powerful engineering idea:

```text
Isolate work.

Compress results.

Preserve context.

Scale through specialized workers.
```

---