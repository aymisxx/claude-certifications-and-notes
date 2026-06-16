# Claude Platform 101 Completion Notes

**Date:** 2026-06-15
**Course:** Anthropic SkillJar - Claude Platform 101
**Status:** Completed
**Quiz Score:** 6/6 - 100%
**Quiz Time:** 2 minutes

# Overview

Today I completed the Anthropic SkillJar **Claude Platform 101** course as part of the broader objective of extracting reusable agentic platform concepts for the **Force Agentic Command Lab** repository.

The course focused on the Claude Developer Platform as Anthropic's programmable infrastructure for building with Claude through APIs, SDKs, command-line tools, console workflows, managed agents, tools, skills, MCP, context management, and production controls.

The primary value of the course was not only learning how to call Claude from code, but understanding how Claude becomes a software subsystem inside real products.

The course covered the transition from:

```text
Claude As Chat Interface
→
Claude As API Component
→
Claude As Tool-Using Agent
→
Claude As Managed Runtime
→
Claude As Production Agent Infrastructure
```

The main engineering themes were:

- API-level access to Claude.
- the `messages.create` request pattern.
- model selection under cost-quality-latency constraints.
- agent loops.
- custom tool use.
- built-in server and client tools.
- SDK tool runners.
- extended thinking.
- Skills as reusable procedures.
- MCP as third-party integration infrastructure.
- context management for long-running agents.
- managed agents as hosted agent loops.
- environments, sessions, and event streams.
- rubrics, graders, permissions, memory, and multi-agent coordination.
- using Claude Code to generate Claude API integrations.

The course made clear that the Claude Platform is not just an API endpoint.

It is a layered agentic application platform.

---

# Curriculum Covered

## What Is the Claude Developer Platform?

- Claude Developer Platform overview.
- REST API.
- SDKs.
- Command-line interfaces.
- Claude Console.
- API keys.
- usage monitoring.
- managed agents.
- prompt testing.
- three-layer platform model.
- primitives, infrastructure, and controls.
- help desk reply drafting example.
- `messages.create` basics.

## Your First API Call

- API key setup.
- `.env.local` secret management.
- SDK installation.
- `messages.create`.
- model parameter.
- `max_tokens`.
- messages array.
- system prompts.
- response content blocks.
- code review example.
- script-to-product transition.

## Choosing the Right Model

- model tier trade-offs.
- Haiku.
- Sonnet.
- Opus.
- Fable.
- cost-quality-latency trade-offs.
- representative eval sets.
- `response.usage`.
- token-based billing.
- model routing by task type.

## The Agent Loop Explained

- agentic workflows.
- Claude in a loop.
- tool selection.
- tool execution.
- tool result feedback.
- `stop_reason`.
- `tool_use`.
- `end_turn`.
- weather tool example.
- production compliance agent example.

## What Is Tool Use?

- tools as functions exposed to Claude.
- tool schemas.
- tool names.
- tool descriptions.
- input schemas.
- tool result blocks.
- multiple-tool dispatch.
- SDK tool runner.
- custom tools versus delegated loop execution.

## What Is Thinking?

- extended thinking.
- adaptive thinking.
- effort levels.
- thinking blocks.
- trade-off-heavy reasoning.
- when to use thinking.
- when to skip thinking.
- production reasoning examples.

## Built-In Tools

- server tools.
- web search.
- code execution.
- web fetch.
- client tools.
- memory.
- bash.
- `server_tool_use` blocks.
- code execution result blocks.
- no manual loop for server tools.

## Skills

- Skills as reusable procedures.
- `SKILL.md`.
- instructions, scripts, and resources.
- tools versus Skills.
- progressive Skill loading.
- uploading Skills.
- attaching Skills through `container.skills`.
- pairing Skills with code execution.
- status report generator example.

## MCP

- MCP as third-party integration infrastructure.
- integration maintenance problem.
- tools versus Skills versus MCP.
- `mcp_servers`.
- `mcp_toolset`.
- MCP introspection.
- scoped tool access.
- read-only MCP access patterns.
- beta MCP connector.

## Context Management

- context window as finite input state.
- system prompt, messages, tools, files, Skills, and thinking blocks.
- just-in-time context.
- server-side compaction.
- prompt caching.
- memory tool.
- layering context patterns.

## What Are Managed Agents?

- managed agents as hosted agent loops.
- isolated containers.
- file system access.
- bash execution.
- web search.
- sessions.
- environments.
- custom tools.
- MCP connections.
- memory.
- outcomes.
- rubrics and graders.
- permissions policies.
- multi-agent coordination.

## Building Your First Managed Agent

- agent primitive.
- environment primitive.
- session primitive.
- event primitive.
- `client.beta.agents.create`.
- `client.beta.environments.create`.
- `client.beta.sessions.create`.
- event stream.
- kickoff event.
- `agent.message`.
- `agent.tool_use`.
- `session.status_idle`.

## Building With Claude Code

- Claude Code as API-integration assistant.
- built-in Claude API Skill.
- `/claude-api`.
- `AnthropicsSkills` marketplace plugin.
- stub-first API development.
- tool runner implementation.
- review-diff workflow.

---

# Key Concepts Learned

## 1. Claude Developer Platform

The Claude Developer Platform is Anthropic's infrastructure for using Claude programmatically.

Instead of interacting through a browser chat interface, developers send structured requests from application code and receive structured responses back.

```text
Application Code
→ Claude API
→ Structured Response
→ Product Feature
```

The platform includes:

```text
REST API
SDKs
CLIs
Claude Console
API Key Management
Usage Monitoring
Prompt Testing
Managed Agents
Analytics
```

The platform turns Claude from an assistant UI into a controllable software component.

---

## 2. The Three Platform Layers

The course frames the platform as three stacked layers.

```text
Primitives
↓
Infrastructure
↓
Controls
```

Primitives are the callable building blocks.

```text
Messages API
Tool Use
Files
Web Search
Code Execution
MCP Servers
Skills
```

Infrastructure supports scaled agentic systems.

```text
Managed Agents
Retries
Queues
Observability
Prompt Caching
Memory
```

Controls support production management.

```text
Dashboards
Evaluations
Workspaces
Usage Limits
Spend Limits
Request Logs
```

The shorthand is:

```text
Build With Primitives.
Scale On Infrastructure.
Run With Control.
```

---

## 3. Claude Inside a Product

The help desk example demonstrates the core product pattern.

```text
User Opens Ticket
→ App Retrieves Ticket Content
→ Backend Calls messages.create
→ Claude Drafts Reply
→ Draft Appears In UI
→ Human Reviews Before Sending
```

This is not building a chatbot from scratch.

It is adding Claude into an existing product workflow.

```text
Claude In Browser
=
Conversation Partner

Claude Through API
=
Programmable Product Subsystem
```

---

## 4. Messages API

The central API call is:

```text
messages.create
```

A minimal useful request includes:

```text
model
max_tokens
messages
```

A richer product request often includes:

```text
system
tools
files
thinking
container
mcp_servers
context_management
```

Example shape:

```python
response = client.messages.create(
    model="claude-haiku-4-5",
    max_tokens=1024,
    system=TONE_AND_GUIDELINES,
    messages=[
        {"role": "user", "content": ticket_content}
    ],
)
```

Each field has a clear role.

```text
model
→ compute / quality tier

max_tokens
→ output budget

system
→ behavior policy and role

messages
→ task input and conversation state

response.content
→ structured output blocks
```

---

## 5. API Key Handling

API keys should be stored outside source code.

```text
Use:
.env.local

Avoid:
Hardcoded API keys in source files
```

Hardcoded keys can leak through version control.

The engineering rule is simple:

```text
Secrets Belong In Configuration, Not Code
```

---

## 6. Response Content Blocks

Claude API responses are structured as content blocks, not plain strings.

A basic response may contain text blocks.

More advanced responses may contain:

```text
text
tool_use
server_tool_use
thinking
code execution results
```

Therefore production code should loop over blocks and branch on `block.type`.

```javascript
for (const block of response.content) {
  if (block.type === "text") {
    console.log(block.text);
  }
}
```

This is API schema discipline.

```text
Never Assume Output Is A Plain String.
Parse By Declared Block Type.
```

---

## 7. Model Selection

Model choice affects:

```text
quality
latency
cost
throughput
```

The course frames model selection as a cost-quality trade-off.

Bad default:

```text
Always Use The Smartest Model
```

This wastes money and latency on simple tasks.

Also bad:

```text
Always Use The Cheapest Model
```

This may fail quality requirements.

The correct rule is:

```text
Use The Cheapest Model Whose Output You Would Actually Ship.
```

---

## 8. Model Tiers

Haiku is fastest and lowest cost.

Use it for:

```text
classification
extraction
routing
simple formatting
high-volume simple work
```

Sonnet is the balanced default.

Use it for:

```text
most production work
drafting
moderate reasoning
analysis
general assistant features
```

Opus is higher capability, slower, and more expensive.

Use it for:

```text
deep reasoning
complex analysis
multi-step coding
nuanced writing
high-stakes synthesis
```

Fable is described as an even higher capability tier above Opus and should be reserved for the hardest tasks where the extra cost is justified.

---

## 9. Evaluation Before Production

Before choosing a model, run a simple evaluation.

The course recommends:

```text
20 to 30 representative examples
```

The process is:

```text
Run Examples Through Haiku
→ If Quality Is Shippable, Stop
→ Otherwise Try Sonnet
→ Otherwise Try Opus
→ Reserve Fable For Rare Hard Cases
```

The model-selection objective is:

```text
minimize cost + latency
subject to quality ≥ required threshold
```

---

## 10. Usage and Billing

The API returns usage data.

```python
print(response.usage)
```

This includes input and output tokens.

Cost roughly follows:

```text
Total Cost
=
Σ [input_tokens × input_rate(model) + output_tokens × output_rate(model)]
```

Therefore production systems should track both model choice and prompt size.

---

## 11. Model Routing

Production systems should route different tasks to different models.

Example:

```text
Incoming File Classification
→ Haiku

Client Update Draft
→ Sonnet

RFP Response
→ Opus
```

A single product can use multiple models inside the same workflow.

```text
Task Type
→ Complexity Estimate
→ Model Selection
→ messages.create
→ Result
```

---

## 12. Agent Loop

An agent is Claude running inside a loop.

```text
Observe
→ Decide
→ Act
→ Read Result
→ Repeat
```

API version:

```text
Send messages with tools
→ Claude returns final answer or tool_use
→ Code executes requested tool
→ Code sends tool_result back
→ Repeat until stop_reason == end_turn
```

The central control signal is `stop_reason`.

```text
tool_use
→ run the tool and continue

end_turn
→ return final answer and stop
```

---

## 13. Minimal Agent State Machine

The simplest agent loop can be represented as:

```text
START
→ CALL_CLAUDE
→ if tool_use:
      RUN_TOOL
      APPEND_TOOL_RESULT
      LOOP
→ if end_turn:
      RETURN_FINAL_ANSWER
```

This is the basic topology behind agentic workflows.

Two API calls, one tool execution, and one final answer are enough to demonstrate the loop.

---

## 14. Tool Use

A tool is a function that the application exposes to Claude.

```text
Tool
=
Named External Capability With Schema
```

A tool definition has:

```text
name
description
input_schema
```

Claude does not execute the tool itself.

```text
Claude Requests.
Your Code Executes.
Claude Observes The Result.
```

This boundary is essential.

Tool use is controlled delegation, not uncontrolled autonomy.

---

## 15. Tool Descriptions

Tool descriptions are control surfaces.

Claude reads descriptions to decide:

```text
Should I call this tool?
When should I call it?
What input should I provide?
```

Vague descriptions cause poor tool selection.

Bad description:

```text
Gets stuff.
```

Good description:

```text
Look up a specific building code section by its identifier. Returns the full text of that code section.
```

Specific descriptions produce better tool behavior.

---

## 16. Tool Result Blocks

When Claude requests a tool, the response contains a `tool_use` block.

The application executes the tool and sends back a `tool_result` block.

The result is tied to the tool call ID.

```text
tool_use block:
  id
  name
  input

tool_result block:
  tool_use_id
  content
```

The ID acts as a correlation handle.

This matters when Claude requests multiple tools.

---

## 17. Multiple Tools

Claude can choose among multiple tools.

Example:

```text
get_weather
→ current weather

get_forecast
→ next few days forecast
```

Your code dispatches on tool name.

```javascript
function runTool(name, input) {
  switch (name) {
    case "get_weather":
      return getWeather(input.city);
    case "get_forecast":
      return getForecast(input.city);
  }
}
```

Adding a manual tool usually means:

```text
Add Tool Schema
+
Add Dispatcher Branch
```

This works but becomes boilerplate-heavy.

---

## 18. Tool Runner

The SDK tool runner reduces boilerplate.

It can handle:

```text
while loop
stop_reason switch
tool schemas
tool_result construction
message history updates
```

Example shape:

```javascript
const runner = client.beta.messages.toolRunner({
  model: "claude-sonnet-4-6",
  max_tokens: 1024,
  messages: [...],
  tools: [getWeather, getForecast],
});

const finalMessage = await runner.untilDone();
```

The tool runner is useful when the developer wants less loop wiring but still owns the tools and product integration.

---

## 19. Manual Loop, Tool Runner, Managed Agents

The course establishes a responsibility ladder.

```text
Manual Agent Loop
→ You own loop, tools, execution, and state.

Tool Runner
→ SDK owns loop mechanics.
→ You own tools and product integration.

Managed Agents
→ Anthropic owns more of the agent runtime.
→ You configure behavior and consume outputs.
```

As autonomy increases, the specification must become sharper.

---

## 20. Extended Thinking

Extended thinking gives Claude additional reasoning time before answering.

It is useful for:

```text
math
multi-step logic
code debugging
regulatory analysis
trade-off comparisons
cross-document reasoning
```

It should be skipped for:

```text
classification
simple extraction
basic routing
boilerplate generation
```

because it adds latency and token cost without meaningful benefit for simple tasks.

---

## 21. Adaptive Thinking

With Opus 4.7, thinking is adaptive.

Enable it with:

```python
thinking={"type": "adaptive"}
```

Control depth with the `effort` parameter inside `output_config`.

```python
output_config={"effort": "high"}
```

Effort levels:

```text
low
medium
high
xhigh
max
```

Important placement detail:

```text
effort belongs inside output_config,
not beside the thinking block.
```

---

## 22. Thinking Blocks

With extended thinking enabled, responses may include:

```text
thinking blocks
tool_use blocks
text blocks
```

This reinforces the content-block parsing rule.

```text
Always Inspect block.type.
```

Thinking is an internal compute budget.

```text
More Thinking
→ Higher Reasoning Depth
→ Higher Latency And Cost
```

---

## 23. Built-In Tools

Anthropic provides built-in tools for common capabilities.

There are two categories:

```text
Server Tools
→ declared by developer, run by Anthropic

Client Tools
→ run where developer code runs, with SDK support
```

Server tools include:

```text
web search
code execution
web fetch
```

Client tools include:

```text
memory
bash
```

---

## 24. Server Tools

Server tools run on Anthropic infrastructure.

They are declared in the tools array.

Example:

```python
tools=[
    {"type": "web_search_20260209", "name": "web_search"}
]
```

or:

```python
tools=[
    {"type": "code_execution_20260120", "name": "code_execution"}
]
```

For server tools:

```text
No Manual Agent Loop Required
No stop_reason Switch Required
No Manual tool_result Injection Required
```

Anthropic runs the tool and returns the result in the same response.

---

## 25. Built-In Tool Response Blocks

Built-in server tools produce additional response block types.

Examples:

```text
server_tool_use
bash_code_execution_tool_result
text
```

The same parsing rule applies:

```text
Loop Over Content Blocks.
Branch On block.type.
```

---

## 26. Client Tools

Client tools run where your code runs, but the SDK provides the schema and a sensible runner.

Examples:

```text
memory
bash
```

The distinction is:

```text
Server Tool
→ Anthropic Runs It

Client Tool
→ Your Environment Runs It
```

---

## 27. Skills

Skills are folders of instructions, scripts, and resources that Claude loads dynamically for specialized tasks.

The core file is:

```text
SKILL.md
```

A Skill teaches Claude how to perform a task according to a reusable procedure.

Examples:

```text
status report format
review checklist
release notes procedure
spreadsheet workflow
document formatting rules
```

---

## 28. Tools vs Skills

Tools and Skills solve different problems.

```text
Tools
→ What Claude Can Do

Skills
→ How Claude Should Do It
```

Examples:

```text
Tool:
  Look up this code section.

Skill:
  Generate a daily status report using this team's exact structure.
```

Tools connect Claude to external actions and data.

Skills teach Claude a procedure.

---

## 29. Progressive Skill Loading

Skills do not fully load into context immediately.

At startup, only the Skill's name and description are loaded.

The full Skill loads when Claude decides it is relevant.

```text
Many Skills Available
→ Lightweight Metadata Loaded
→ Full Skill Loaded Just In Time
```

This protects the context window.

---

## 30. Uploading and Attaching Skills

Skills are uploaded once and referenced by ID.

```python
skill = client.beta.skills.create(
    display_title="Status Report Generator",
    files=files_from_dir("status-report-skill"),
)
```

They are attached through `container.skills`.

```python
container={
    "skills": [
        {
            "type": "custom",
            "skill_id": skill.id,
            "version": "latest",
        }
    ]
}
```

Skills can be layered because `container.skills` is a list.

Skills often pair well with code execution when the procedure needs to run scripts or process files.

---

## 31. MCP

MCP solves the third-party integration maintenance problem.

Without MCP, developers must build and maintain wrappers for every third-party service.

```text
Asana Wrapper
Slack Wrapper
Google Calendar Wrapper
Linear Wrapper
```

With MCP, the service provider or integration maintainer exposes a standard MCP server.

```text
Service Provider Maintains MCP Server.
Application Connects Through Standard Protocol.
Claude Discovers Tools And Schemas.
```

When the third-party API changes, the server maintainer updates the MCP server.

The application does not rewrite its own wrapper.

---

## 32. Tools vs Skills vs MCP

The distinction:

```text
Tools
→ Your internal systems and data

Skills
→ Your reusable processes

MCP
→ Third-party services and integrations
```

Short version:

```text
Tools Are For Your Stuff.
Skills Are For Your Process.
MCP Is For Everyone Else's Stuff.
```

---

## 33. Connecting to MCP Servers

MCP connections use two pieces.

```text
mcp_servers
→ declares connection details

mcp_toolset
→ grants Claude access to tools from that server
```

Example shape:

```python
response = client.beta.messages.create(
    model="claude-opus-4-8",
    max_tokens=1000,
    messages=[
        {"role": "user", "content": "What tools do you have available?"}
    ],
    mcp_servers=[
        {
            "type": "url",
            "url": "https://mcp.linear.app/mcp",
            "name": "linear",
            "authorization_token": os.environ["LINEAR_MCP_TOKEN"],
        }
    ],
    tools=[
        {
            "type": "mcp_toolset",
            "mcp_server_name": "linear",
        }
    ],
    betas=["mcp-client-2025-11-20"],
)
```

Claude introspects the MCP server and discovers tools and schemas automatically.

No manual tool schemas are required.

---

## 34. Filtering MCP Tools

MCP servers may expose many tools.

Access should be scoped.

Pattern:

```python
tools=[
    {
        "type": "mcp_toolset",
        "mcp_server_name": "slack",
        "default_config": {
            "enabled": False,
        },
        "configs": {
            "search_messages": {"enabled": True},
            "list_channels": {"enabled": True},
        },
    }
]
```

This enables least-privilege access.

```text
Disable Everything By Default.
Enable Only Needed Tools.
```

This is especially useful for read-only access.

---

## 35. Context Management

Context is everything Claude sees on a turn.

It includes:

```text
system prompt
message history
tool definitions
tool results
attached files
Skills
thinking blocks
```

The context window is finite and not free.

The goal is not to include everything.

The goal is:

```text
Fit The Right Things In.
```

---

## 36. Four Context Management Patterns

The course describes four patterns.

```text
Just-In-Time Context
Server-Side Compaction
Prompt Caching
Memory Tool
```

Each solves a different failure mode.

---

## 37. Just-In-Time Context

Just-in-time context means loading only what the agent needs now.

Do not stuff everything into the initial prompt.

Example:

```text
Bad:
  Put entire building code book in system prompt.

Good:
  Give Claude a lookup_building_code tool.
  Retrieve specific sections when needed.
```

This is a design pattern, not a special API feature.

---

## 38. Server-Side Compaction

Server-side compaction summarizes old turns into a compact block when the conversation grows long.

Enable with:

```python
response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    context_management={
        "edits": [
            {"type": "compact"}
        ]
    },
    messages=messages,
)
```

The API handles summarization automatically when the input crosses the trigger threshold.

---

## 39. Prompt Caching

Prompt caching reuses stable request parts at lower cost.

Good candidates:

```text
system prompt
tool definitions
long reference document
stable policies
stable project instructions
```

If a 4,000-token system prompt is reused 100 times per hour, caching avoids paying full repeated input cost every time.

Engineering rule:

```text
Cache Stable Context.
Retrieve Dynamic Context Just In Time.
```

---

## 40. Memory Tool

The memory tool persists context across sessions.

Claude reads and writes a memory directory through tool calls.

The storage backend is owned client-side.

Examples:

```text
filesystem
database
encrypted store
custom backend
```

Use memory for:

```text
user preferences
long-running project notes
decisions from prior sessions
agent working notes
recurring customer context
```

---

## 41. Layering Context Patterns

Production agents usually combine all four context strategies.

Example:

```text
Prompt Caching
→ cache system prompt and tool definitions

Just-In-Time Context
→ fetch only relevant building code sections

Compaction
→ summarize long review traces

Memory
→ persist prior decisions and user preferences
```

Each pattern solves a distinct problem.

```text
Just-In-Time Context
→ context bloat

Compaction
→ long conversation overflow

Prompt Caching
→ repeated stable-token cost

Memory
→ cross-session continuity
```

---

## 42. Managed Agents

Managed agents are hosted, stateful agent loops.

Developers define:

```text
agent persona
model
system prompt
toolset
environment
memory
permissions
rubric
success criteria
```

Anthropic runs the loop inside an isolated container.

```text
Managed Agent
=
Hosted Agent Loop + Sandbox + Tools + Events + State
```

---

## 43. Managed-Agent Building Blocks

Managed agents are built from four primitives.

```text
Agent
Environment
Session
Events
```

Agent:

```text
Reusable persona, model, prompt, and toolset.
```

Environment:

```text
Sandbox where the agent runs.
```

Session:

```text
A single run of an agent inside an environment.
```

Events:

```text
Messages, tool uses, results, status updates, and replies.
```

The shift is:

```text
You Do Not Run A While Loop.
You Send Events And Read Events.
```

---

## 44. Creating a Managed Agent

Example:

```python
agent = client.beta.agents.create(
    name="Line Counter",
    model="claude-opus-4-8",
    system="You are a helpful agent that completes small file tasks.",
    tools=[
        {"type": "agent_toolset_20260401", "default_config": {"enabled": True}}
    ],
)
```

The agent is reusable across sessions.

The bundled agent toolset gives file, bash, and web capabilities.

---

## 45. Creating an Environment

Example:

```python
environment = client.beta.environments.create(
    name="line-counter-env",
    config={
        "type": "cloud",
        "networking": {"type": "unrestricted"},
    },
)
```

The environment defines where the work happens.

```text
Agent
→ Who Is Working

Environment
→ Where The Work Happens
```

---

## 46. Creating a Session

Example:

```python
session = client.beta.sessions.create(
    agent=agent.id,
    environment_id=environment.id,
    title="Count lines demo",
)
```

The session is the unit of work.

It binds:

```text
agent
environment
task run
```

---

## 47. Event Streams

The event stream should be opened before sending the kickoff message.

```text
Open Event Stream First.
Then Send Kickoff Event.
```

Reason:

```text
The stream only delivers events that occur after it opens.
```

Important event types:

```text
agent.message
agent.tool_use
session.status_idle
```

Meanings:

```text
agent.message
→ Claude produced text

agent.tool_use
→ Claude selected a tool

session.status_idle
→ active work is done
```

---

## 48. Managed Agents and Rubrics

Rubrics define what done means.

Example:

```text
Lighthouse score above 90
No render-blocking resources
All images lazy loaded
```

A separate grader can evaluate output against the rubric.

Claude can then read feedback, revise, and resubmit.

This makes managed agents closer to closed-loop optimization than one-shot generation.

```text
Agent Work
→ Grader Feedback
→ Agent Revision
→ Rubric Pass
```

---

## 49. Multi-Agent Coordination

Managed agents can support coordination between specialists.

Example incident response structure:

```text
Coordinator Agent
→ Diagnostics Specialist
→ Log Analysis Specialist
→ Communications Specialist
→ Past Incident Memory Search
```

Each specialist can run in its own context window while sharing a filesystem.

The coordinator synthesizes the final result.

Human approval can gate sensitive actions such as sending Slack messages.

---

## 50. Claude Code for API Development

Claude Code can help implement Claude API integrations.

The recommended workflow:

```text
Stub File
→ Prompt Claude Code With File + Pattern + End State
→ Claude Code Writes Code
→ Claude Code Runs It
→ Claude Code Patches Errors
→ User Reviews Diff
```

Claude Code includes a built-in Claude API Skill.

Invoke it with:

```text
/claude-api
```

If missing, add the plugin:

```bash
/plugin marketplace add AnthropicsSkills
```

The recurring Claude API code shape is:

```text
Define A Tool.
Hand It To A Runner.
Return The Result.
```

---

# Course-Specific Sections

## Claude Platform as Product Infrastructure

The Claude Platform moves Claude from a manual chat interface into product infrastructure.

```text
Button Click
→ Backend Route
→ Claude API Call
→ Structured Output
→ UI Render Or Database Update
```

This pattern is useful for:

```text
support reply drafting
meeting summaries
code review
compliance checks
proposal fact-checking
research reports
file cleanup
incident summaries
```

The product boundary matters:

```text
Claude Suggests.
Application Validates.
Human Approves Where Needed.
```

---

## Agent Loop as Runtime Skeleton

The agent loop is the skeleton underneath tool-using applications.

```text
messages
+ tools
+ stop_reason
+ tool_result
+ repeated API calls
=
manual agent runtime
```

The platform provides multiple levels of abstraction over this skeleton:

```text
Manual Loop
→ Full control

Tool Runner
→ SDK handles loop plumbing

Managed Agents
→ Anthropic hosts the loop
```

---

## Tools, Skills, and MCP as Capability Types

The course clarifies a clean taxonomy.

```text
Tools
→ Actions and data from systems you control

Skills
→ Procedures and formats you want Claude to follow

MCP
→ Third-party integrations maintained outside your codebase
```

This is a practical design rule for agent architecture.

---

## Context Management as State Discipline

Long-running agents are constrained by context.

Context management is not optional once agents become stateful.

The platform provides several tools:

```text
Just-In-Time Retrieval
Compaction
Caching
Memory
```

The design goal is:

```text
Keep Active Context Relevant, Not Huge.
```

A good agent does not drag its entire history and all possible documents into every step.

It retrieves, summarizes, caches, or persists depending on the type of information.

---

## Managed Agents as Hosted MUSP

Managed agents are a hosted version of the same loop used throughout the Force Agentic Command Lab.

```text
Observe
→ Act
→ Verify
→ Update State
→ Repeat
```

The difference is that Anthropic hosts the runtime, sandbox, and event stream.

Developers define the mission and observe the trace.

---

# Force Agentic Command Lab Translation

The course can be summarized as:

```text
Claude Platform 101
=
From Prompting To Programmable Agent Infrastructure
```

It reinforces the Force Agentic Command Lab's central idea:

```text
Good AI workflows are engineered interfaces,
not magical wording.
```

The platform offers the technical substrate for converting command patterns into deployed systems.

---

## 1. API Calls as Command Interfaces

A good API request is a command interface.

```text
Goal
→ What output is needed?

System
→ What role and rules govern behavior?

Messages
→ What context is currently observed?

Model
→ What cost-quality tier should process it?

Max Tokens
→ What output budget is allowed?

Tools
→ What actions are available?

Validation
→ What makes output acceptable?
```

This maps directly to reusable command templates.

---

## 2. Agent Loops as Executable Workflows

The agent loop turns a vague task into an executable workflow.

```text
Task
→ Reason
→ Tool Call
→ Tool Result
→ Updated State
→ Repeat
→ Final Answer
```

For the lab, this becomes:

```text
Do not ask an assistant to "be agentic."
Define the loop, tools, stop condition, and validation rule.
```

---

## 3. Tool Design as Interface Design

Every tool should have:

```text
clear name
specific description
strict input schema
predictable output
failure behavior
authorization boundary
```

Tool descriptions are not documentation decoration.

They affect action selection.

```text
Tool Description
=
Control Surface For The Agent
```

---

## 4. Context Placement Rule

Different kinds of information belong in different places.

```text
Stable instructions
→ system prompt or cached prefix

Reusable procedure
→ Skill

External action
→ tool

Third-party integration
→ MCP

Long-term state
→ memory

Old conversation
→ compaction

Large reference material
→ just-in-time retrieval

Common hosted capability
→ built-in tool
```

This avoids prompt bloat and makes workflows easier to maintain.

---

## 5. Model Routing Rule

Model selection should be systematic.

```text
Simple / high-volume
→ Haiku

Default production
→ Sonnet

Complex / high-stakes
→ Opus

Extreme capability
→ Fable
```

The rule is:

```text
Use The Weakest Model That Clears The Acceptance Test.
```

---

## 6. Managed Agent Specification Rule

A managed agent should be specified with:

```text
Agent:
  persona, model, system prompt, toolset

Environment:
  packages, files, network access, sandbox constraints

Session:
  specific unit of work

Inputs:
  kickoff event and task context

Tools:
  allowed actions

Memory:
  what prior state to read/write

Permissions:
  what needs approval

Rubric:
  measurable definition of done

Events:
  progress and trace outputs

Stop:
  completion condition
```

This is the Force Lab version of deployment discipline.

---

# Relationship to the Markovian Unit-Step Protocol

Claude Platform 101 maps directly into the Markovian Unit-Step Protocol.

MUSP is:

```text
Observe
→ Act
→ Verify
→ Update State
→ Repeat
```

The course provides API mechanisms for each part of that loop.

---

## Messages API in MUSP

```text
Observe:
  Gather user input, system prompt, files, and current message history.

Act:
  Call messages.create.

Verify:
  Parse response blocks and check output quality.

Update State:
  Save response, usage, and product state.

Repeat:
  Continue if the workflow requires additional turns.
```

Formal view:

```text
s_t = application state
o_t = current prompt/context
a_t = messages.create(model, system, messages)
y_t = response.content
c_t = response.usage
s_t+1 = update(s_t, y_t, c_t)
```

---

## Agent Loop in MUSP

```text
Observe:
  Claude reads current messages and available tools.

Act:
  Claude chooses final answer or tool call.

Verify:
  Code executes the tool and returns a result.

Update State:
  Append assistant tool_use and user tool_result messages.

Repeat:
  Continue until stop_reason == end_turn.
```

Formal loop:

```text
s_t = message history + available tools + external state summary

a_t = π_Claude(s_t)

if a_t = tool_use:
    y_t = Tool(a_t)
    s_t+1 = s_t ∪ {a_t, y_t}

if a_t = end_turn:
    return final answer
```

---

## Thinking in MUSP

Thinking modifies the reasoning policy.

```text
π_Claude(s_t; effort=e)
```

Higher effort gives more internal computation before action selection.

Optimization view:

```text
choose effort e ∈ {low, medium, high, xhigh, max}

maximize expected quality
subject to latency and token budget
```

Thinking is not free intelligence.

It is compute allocation.

---

## Context Management in MUSP

```text
Observe:
  Current context size and relevance.

Act:
  Retrieve, compact, cache, or memorize.

Verify:
  Ensure the active context contains what the next step needs.

Update State:
  Continue with leaner and more relevant context.
```

Context objective:

```text
maximize relevance R(s_t)
minimize token cost T(s_t)
subject to T(s_t) ≤ C_max
```

or:

```text
s_t* = argmax [R(s_t) - λT(s_t)]
```

---

## Managed Agents in MUSP

Managed agents externalize the MUSP loop into hosted infrastructure.

```text
Observe:
  Agent reads session, environment, memory, and task input.

Act:
  Agent calls tools, writes files, runs bash, or searches web.

Verify:
  Tool results, event streams, graders, or permission policies check outcome.

Update State:
  Session state, files, memory, and event log update.

Repeat:
  Runtime continues until idle, rubric pass, or human intervention.
```

Formal view:

```text
s_t = session state + environment state + memory state
a_t = agent-selected action
y_t = tool result or event
v_t = grader / permission / user feedback
s_t+1 = update(s_t, a_t, y_t, v_t)
```

Managed agents are MUSP hosted by the platform.

---

# Personal Additions

## 1. Claude Platform as Agent Operating System

The course reveals Claude Platform as more than an API.

It behaves like an agent operating system.

```text
Messages API
→ process invocation

Tools
→ system calls

Skills
→ reusable programs / procedures

MCP
→ external device drivers

Memory
→ persistent storage

Context Management
→ memory management

Managed Agents
→ hosted worker processes

Events
→ telemetry stream

Rubrics
→ success tests

Permissions
→ safety gates
```

This is the cleanest mental model.

---

## 2. Tools as Sensors and Actuators

Tools can be sensors or actuators.

```text
Sensor Tools:
  read file
  search web
  query database
  lookup code section

Actuator Tools:
  write file
  send email
  create ticket
  run simulation
```

Claude emits structured action requests.

The application or platform executes them under constraints.

This maps naturally to robotics.

---

## 3. Skills as Controllers

Skills are reusable control policies.

They define how Claude should behave for a specialized class of tasks.

```text
Skill
=
Reusable Procedure Controller
```

A status report Skill is like a controller that maps messy activity logs into a standardized report format.

---

## 4. MCP Servers as ROS Drivers

MCP servers remain best understood as ROS-style drivers for AI-accessible integrations.

```text
MCP Server
→ Standard Interface To External Capability
```

They let Claude use third-party systems without every app developer writing and maintaining wrappers.

---

## 5. Context as Working Memory State

Context management is working-memory engineering.

A large context window does not remove the need for state discipline.

```text
Big Context
≠
Good Context
```

Relevant, timely, compressed, and retrievable state matters more than raw size.

---

## 6. Managed Agents as Cloud Robot Workers

Managed agents are similar to deploying a robot worker into a sandboxed cloud workcell.

```text
Agent
→ robot/controller

Environment
→ workcell/simulation arena

Session
→ mission run

Tools
→ sensors and actuators

Events
→ telemetry

Rubric
→ mission success criteria

Grader
→ independent evaluator

Permissions
→ safety interlocks
```

The developer defines the workcell and success condition.

Claude operates inside it.

---

## 7. Specification Quality Increases With Autonomy

The more autonomy an agent has, the sharper the specification must be.

A one-shot prompt can be loose.

A long-running managed agent needs:

```text
tool boundaries
environment constraints
memory policy
permission gates
rubric
event visibility
stop condition
```

Otherwise the agent can drift, loop, or act too broadly.

```text
Autonomy Without Boundaries
=
Operational Risk
```

---

# Overall Assessment

## Strengths

- Clear explanation of the Claude Developer Platform as programmable infrastructure.
- Strong three-layer model: primitives, infrastructure, and controls.
- Practical explanation of `messages.create`.
- Useful first API call example.
- Strong emphasis on API key handling and not leaking secrets.
- Clear treatment of response content blocks.
- Practical model-selection guidance based on evals and cost.
- Strong introduction to agent loops and `stop_reason`.
- Good explanation of tool definitions, schemas, and descriptions.
- Useful distinction between custom tools, built-in tools, tool runners, and managed agents.
- Strong explanation of extended thinking and when to use it.
- Clear distinction between tools, Skills, and MCP.
- Strong context management section.
- Managed agents section gives a useful production architecture model.
- Claude Code section connects platform usage back to practical development workflow.

## Weaknesses

- The course is broad and necessarily compresses many topics.
- Security is introduced but not deeply formalized.
- Tool authorization and write-action approval deserve deeper treatment.
- Cost estimation is discussed conceptually but not turned into a full budgeting framework.
- Evals are recommended, but detailed rubric design is not deeply covered.
- Managed agents are introduced conceptually, but production failure modes are not deeply explored.
- Multi-agent coordination is shown through examples, but scheduling, conflict resolution, and shared-state hazards are not deeply covered.
- Context management patterns are strong, but implementation details for memory backends and compaction validation remain high-level.
- MCP connector usage is shown, but authentication and least-privilege policies could use deeper hardening.

---

# Quiz Results

The final assessment contained six questions.

Results:

```text
Correct Answers: 6 / 6
Score: 100%
Elapsed Time: 2 minutes
```

Topics tested:

- required parameters for `messages.create`.
- who executes custom tools.
- why context management matters for long-running agents.
- best-fit workloads for managed agents.
- Claude Code slash command for the built-in Claude API Skill.
- core platform concepts around API calls, tools, context, and managed agents.

---

# Final Takeaway

Claude Platform 101 shows how Claude becomes part of a real software system.

The central operating principle is:

```text
Move from prompting Claude manually to engineering Claude-powered workflows through APIs, tools, Skills, MCP, context management, and hosted agent runtimes.
```

The course establishes the platform stack:

```text
Messages API
→ direct programmable Claude calls

Tools
→ external actions and data

Tool Runner
→ SDK-managed tool loop

Built-In Tools
→ hosted common capabilities

Skills
→ reusable procedures

MCP
→ third-party integration layer

Context Management
→ finite working-memory discipline

Managed Agents
→ hosted long-running agent execution

Claude Code
→ agentic development assistant for API integrations
```

The strongest engineering lesson is:

```text
The more autonomous the system becomes, the more explicit its interfaces, tools, state, permissions, and success criteria must be.
```

In one sentence:

```text
Claude Platform 101 turns Claude from a chat assistant into programmable agent infrastructure.
```

---