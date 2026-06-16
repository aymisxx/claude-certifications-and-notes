# Model Context Protocol: Advanced Topics Completion Notes

**Date:** 2026-06-15
**Course:** Anthropic SkillJar - Model Context Protocol: Advanced Topics
**Status:** Completed
**Quiz Score:** 10/10 - 100%
**Quiz Time:** 9 minutes

# Overview

Today I completed the Anthropic SkillJar **Model Context Protocol: Advanced Topics** course as part of the broader objective of extracting reusable agentic infrastructure concepts for the **Force Agentic Command Lab** repository.

The course moved beyond basic MCP tool, resource, and prompt definitions into the deeper infrastructure layer behind production-capable MCP systems.

The primary value of the course was not only learning extra MCP features, but understanding how advanced MCP behavior depends on:

- bidirectional client-server communication.
- server-initiated model access through sampling.
- execution observability through logs and progress notifications.
- filesystem access boundaries through roots.
- JSON message patterns.
- initialization handshakes.
- stdio transport behavior.
- StreamableHTTP transport behavior.
- Server-Sent Events.
- stateful versus stateless HTTP trade-offs.
- streaming versus plain JSON responses.
- production deployment constraints.
- horizontal scaling considerations.

The course made clear that MCP is not just a tool wrapper format.

It is a protocol-level infrastructure system where message direction, transport capability, session state, and deployment topology directly determine which agentic behaviors are possible.

---

# Curriculum Covered

## Sampling

- Sampling as server-initiated model generation.
- Client-owned model access.
- Cost shifting from server to client.
- `ctx.session.create_message`.
- `SamplingMessage`.
- `TextContent`.
- `CreateMessageResult`.
- Sampling callback implementation.
- Client session callback registration.
- Public MCP server cost isolation.

## Logging and Progress Notifications

- Real-time tool execution feedback.
- `Context` argument in MCP tools.
- `context.info()` for log messages.
- `context.report_progress()` for progress updates.
- Client-side logging callbacks.
- Client-side progress callbacks.
- CLI, web, and desktop presentation patterns.
- Long-running tool observability.

## Roots

- Filesystem access boundaries.
- Root objects.
- Roots callback.
- `list_roots` workflow.
- File discovery inside approved directories.
- Authorization before file operations.
- `is_path_allowed()` style access checks.
- Local file security and user-friendly path resolution.

## JSON Message Types

- MCP communication as structured JSON messages.
- Request-result messages.
- Notification messages.
- Client-originated messages.
- Server-originated messages.
- Call Tool Request and Call Tool Result.
- Initialize Request and Initialize Result.
- Progress, logging, tool-list, and resource-update notifications.

## The STDIO Transport

- Client launching server as subprocess.
- JSON over stdin and stdout.
- Same-machine client-server operation.
- Full bidirectional communication.
- Local testing through terminal JSON messages.
- Three-message MCP initialization handshake.
- Four communication scenarios.

## The StreamableHTTP Transport

- Remote MCP servers over HTTP.
- HTTP limitations for server-initiated communication.
- Server-Sent Events as a workaround.
- Stateful session IDs.
- `mcp-session-id` header.
- Primary SSE connection.
- Tool-specific SSE connection.
- Message routing across SSE streams.
- HTTP deployment trade-offs.

## State and StreamableHTTP

- `stateless_http` flag.
- `json_response` flag.
- Stateful versus stateless HTTP behavior.
- Load balancer and horizontal scaling challenges.
- Loss of sampling and notifications in stateless mode.
- Loss of streaming in JSON response mode.
- Development versus production transport testing.

---

# Key Concepts Learned

## 1. Sampling

Sampling allows an MCP server to request language-model generation through the connected MCP client.

The server does not call Claude directly.

Instead:

```text
MCP Server
→ prepares prompt
→ asks MCP Client for model generation
→ MCP Client calls Claude or another model
→ MCP Client returns generated text
→ MCP Server uses the result
```

This shifts language-model integration, credentials, and token costs away from the MCP server and onto the connected client.

This is especially important for public MCP servers.

A public server should not become an unlimited token vending machine for arbitrary users.

```text
Without Sampling
=
Server Owns Model Access + API Key + Cost
```

```text
With Sampling
=
Client Owns Model Access + API Key + Cost
```

The server remains a capability provider and prompt initiator.

The client remains the model-access authority.

---

## 2. Sampling Implementation

On the server side, sampling is initiated from inside a tool using the session context.

```python
@mcp.tool()
async def summarize(text_to_summarize: str, ctx: Context):
    prompt = f"""
    Please summarize the following text:
    {text_to_summarize}
    """

    result = await ctx.session.create_message(
        messages=[
            SamplingMessage(
                role="user",
                content=TextContent(
                    type="text",
                    text=prompt
                )
            )
        ],
        max_tokens=4000,
        system_prompt="You are a helpful research assistant",
    )

    if result.content.type == "text":
        return result.content.text
    else:
        raise ValueError("Sampling failed")
```

On the client side, the application defines a sampling callback.

```python
async def sampling_callback(context, params):
    text = await chat(params.messages)

    return CreateMessageResult(
        role="assistant",
        model=model,
        content=TextContent(type="text", text=text),
    )
```

The callback is passed during session creation.

```python
async with ClientSession(
    read,
    write,
    sampling_callback=sampling_callback
) as session:
    await session.initialize()
```

The server asks for generation.

The client decides how to fulfill it.

---

## 3. Sampling as Delegated Inference

Sampling is best understood as delegated inference.

```text
Server Has Data / Capability
Client Has Model Access
Sampling Connects Them
```

A research MCP server might:

```text
Fetch Wikipedia Articles
→ Build Prompt
→ Request Sampling
→ Receive Summary
→ Return Report
```

The server does not become the language-model runtime.

It remains a tool provider that can request reasoning when needed.

This is a clean separation of capability from cognition.

---

## 4. Logging and Progress Notifications

Logging and progress notifications let an MCP server send execution updates to the client while a tool is running.

Without them:

```text
User Calls Long-Running Tool
→ Silence
→ Final Result
```

With them:

```text
User Calls Long-Running Tool
→ "Starting research..."
→ Progress: 20 / 100
→ "Writing report..."
→ Progress: 70 / 100
→ Final Result
```

This improves user experience and execution transparency.

It also makes long-running tools feel alive rather than frozen.

---

## 5. Tool Context for Logs and Progress

The Python MCP SDK provides a `Context` argument to tool functions.

The context object can send logs and progress updates back to the client.

```python
@mcp.tool(
    name="research",
    description="Research a given topic"
)
async def research(
    topic: str = Field(description="Topic to research"),
    *,
    context: Context
):
    await context.info("About to do research...")
    await context.report_progress(20, 100)

    sources = await do_research(topic)

    await context.info("Writing report...")
    await context.report_progress(70, 100)

    results = await generate_report(sources)

    return results
```

Important methods:

```text
context.info()
→ Send log messages to the client
```

```text
context.report_progress(current, total)
→ Send progress updates to the client
```

---

## 6. Client-Side Notification Handling

The server emits notifications.

The client decides how to display them.

Logging callback:

```python
async def logging_callback(params):
    print(params.data)
```

Progress callback:

```python
async def print_progress_callback(progress, total, message):
    if total is not None:
        percentage = (progress / total) * 100
        print(f"Progress: {progress}/{total} ({percentage:.1f}%)")
    else:
        print(f"Progress: {progress}")
```

The logging callback is attached to the session.

The progress callback is attached to the individual tool call.

```python
async with ClientSession(
    read,
    write,
    logging_callback=logging_callback
) as session:
    await session.initialize()

    await session.call_tool(
        name="add",
        arguments={"a": 1, "b": 3},
        progress_callback=print_progress_callback,
    )
```

This separation is useful:

```text
Logging Callback
→ Session-wide status messages
```

```text
Progress Callback
→ Tool-call-specific completion state
```

---

## 7. Notifications as Tool Telemetry

Logging and progress notifications are telemetry for MCP tools.

```text
Tool Execution
→ State Transition
→ Log / Progress Signal
→ Client UI Update
```

A long-running tool without notifications is a black box.

```text
Input → ??? → Output
```

A tool with notifications is traceable.

```text
Input
→ Fetching sources
→ Parsed 12 / 20 files
→ Generating report
→ Validating output
→ Output
```

Good logs should describe meaningful state transitions.

Bad logs:

```text
Doing thing...
Still doing thing...
Doing more thing...
```

Good logs:

```text
Fetching source documents...
Parsed 12 / 20 documents...
Running summarization...
Validating output schema...
```

---

## 8. Roots

Roots are approved local filesystem locations that an MCP client exposes to an MCP server.

They define where the server is allowed to look and operate.

```text
Approved Root:
~/Movies

Accessible:
~/Movies/biking.mp4

Not Accessible:
~/Documents/private_notes.txt
```

Roots solve two problems at once:

```text
1. File Discovery
2. File Access Control
```

Users should not need to provide full paths every time.

Servers should not be allowed to search the entire filesystem.

Roots give a scoped middle path.

---

## 9. Roots Workflow

Without roots, the user might say:

```text
Convert biking.mp4 to MOV.
```

But Claude may not know where `biking.mp4` lives.

With roots:

```text
User Request
→ Claude Calls list_roots
→ Approved Directories Returned
→ Claude Searches Accessible Directories
→ File Found
→ Tool Called With Full Path
→ Conversion Runs
```

This lets users refer to files naturally while keeping server access bounded.

---

## 10. Roots as Security Boundaries

Roots are a permission boundary.

If the user exposes only the Desktop folder, the server cannot access Downloads, Documents, or other private locations unless explicitly allowed.

However, the MCP SDK does not automatically enforce all root restrictions.

Server developers should implement authorization checks manually.

A typical helper:

```python
from pathlib import Path


def is_path_allowed(path: str, roots: list[str]) -> bool:
    requested = Path(path).resolve()

    for root in roots:
        allowed_root = Path(root).resolve()
        try:
            requested.relative_to(allowed_root)
            return True
        except ValueError:
            continue

    return False
```

Every filesystem operation should check this before acting.

```text
Read File
Write File
List Directory
Move File
Delete File
Convert File
Index Folder
```

The access rule is:

```text
Access allowed iff requested_path ∈ approved_root_subtree
```

Mathematically:

```text
allowed(p) = ∃ r ∈ R such that p is inside r
```

This must use canonical paths, not naive string matching.

---

## 11. JSON Message Types

MCP communication happens through structured JSON messages.

Message types define what is being requested, returned, or announced.

A basic tool call follows:

```text
Call Tool Request
→
Call Tool Result
```

The protocol specification defines the authoritative set of message types.

These types may be written in TypeScript for schema clarity, but MCP messages are exchanged as JSON.

---

## 12. Request-Result Messages

Request-result messages are paired.

A request expects a result.

Examples:

```text
Call Tool Request
→ Call Tool Result

List Prompts Request
→ List Prompts Result

Read Resource Request
→ Read Resource Result

Initialize Request
→ Initialize Result
```

These behave like service calls.

```text
Request
=
Command or Query

Result
=
Returned Output
```

---

## 13. Notification Messages

Notifications are one-way messages.

They do not require a result.

Examples:

```text
Progress Notification
Logging Message Notification
Tool List Changed Notification
Resource Updated Notification
Initialized Notification
Cancelled Notification
```

These behave more like event or telemetry messages.

```text
Notification
=
Something happened; no response required
```

---

## 14. MCP Is Bidirectional

A crucial advanced idea is that MCP is bidirectional.

Both clients and servers can initiate communication.

Client-to-server examples:

```text
call_tool
list_tools
read_resource
list_prompts
get_prompt
```

Server-to-client examples:

```text
sampling request
list roots request
progress notification
logging notification
resource update notification
```

This matters because transport choice determines whether these message directions are actually possible.

The protocol may support bidirectional behavior, but the transport must carry it.

---

## 15. STDIO Transport

The stdio transport runs the MCP server as a subprocess.

The client communicates with the server through standard input and output.

```text
Client
→ writes JSON to server stdin

Server
→ writes JSON to stdout
```

This works when client and server run on the same machine.

It is the simplest transport for local development.

---

## 16. STDIO as the Ideal Local Baseline

Stdio supports full bidirectional MCP communication cleanly.

Four communication scenarios are possible:

```text
Client → Server request
Server → Client response
Server → Client request
Client → Server response
```

This makes stdio excellent for:

- local development.
- debugging.
- testing servers.
- MCP Inspector workflows.
- local filesystem tools.
- full bidirectional MCP behavior.

Its main limitation is deployment scope.

```text
stdio
=
local-only direct process communication
```

---

## 17. MCP Initialization Sequence

Every MCP connection begins with a three-message handshake.

```text
1. Initialize Request
   Client → Server

2. Initialize Result
   Server → Client

3. Initialized Notification
   Client → Server
```

Only after this handshake should normal MCP requests proceed.

This is like bringing up a robot subsystem:

```text
Boot
→ Report Capabilities
→ Confirm Ready
→ Begin Mission
```

---

## 18. StreamableHTTP Transport

StreamableHTTP allows MCP clients to connect to remotely hosted servers over HTTP.

This enables public or network-accessible MCP servers.

```text
Client Machine
→ HTTP
→ Remote MCP Server
```

However, HTTP naturally supports:

```text
Client → Server request
Server → Client response
```

It does not naturally support:

```text
Server → Client request
Client → Server response
```

This is a problem because advanced MCP features rely on server-to-client communication.

Affected features include:

- sampling.
- list roots requests.
- progress notifications.
- logging notifications.
- resource update notifications.
- cancellations.

---

## 19. Server-Sent Events

StreamableHTTP uses Server-Sent Events to work around HTTP's server-to-client limitation.

The client opens a long-lived GET connection.

The server keeps the response open and streams messages back to the client.

```text
Client GET Request
→ Long-Lived SSE Connection
→ Server Streams Messages Back
```

This creates a server-to-client message lane over HTTP.

```text
POST
=
Client-to-server requests

SSE GET
=
Server-to-client streamed messages
```

This allows HTTP-based MCP servers to preserve more of the full bidirectional protocol.

---

## 20. StreamableHTTP Session IDs

After initialization, the server returns a session identifier.

```text
mcp-session-id
```

This ID identifies the client across future HTTP requests.

The sequence is:

```text
Initialize Request
→ Initialize Result + mcp-session-id header
→ Initialized Notification with session ID
→ Future requests include session ID
```

The session ID allows the server to associate separate HTTP requests with the same MCP client session.

---

## 21. Dual SSE Connections

StreamableHTTP can use multiple SSE connections.

The course described two major channels:

```text
Primary SSE Connection
→ long-lived
→ server-initiated requests
→ general server-to-client communication
```

```text
Tool-Specific SSE Connection
→ created for a specific tool call
→ closes when tool result is sent
```

Message routing may look like:

```text
Progress Notifications
→ Primary SSE Connection

Logging Messages + Tool Results
→ Tool-Specific SSE Connection
```

This is more complex than stdio, but it allows remote HTTP deployment while preserving bidirectional MCP features.

---

## 22. `stateless_http=True`

The `stateless_http` flag changes StreamableHTTP behavior significantly.

When enabled:

```text
No session IDs
No tracked client sessions
No GET SSE pathway
No server-to-client requests
No sampling
No progress reports
No subscriptions
Initialization may not be required
```

This simplifies scaling behind load balancers but removes advanced bidirectional behavior.

Use stateless HTTP when:

- horizontal scaling is more important than full MCP functionality.
- tools are simple request-result operations.
- sampling is not needed.
- progress/logging are not needed.
- server-to-client requests are not needed.

---

## 23. Load Balancer State Problem

Stateful StreamableHTTP becomes difficult when horizontally scaled.

Example:

```text
Client
→ Load Balancer
→ Server A handles SSE connection
→ Server B handles POST tool call
```

If Server B needs to send a sampling request or progress message to the client, it must coordinate with Server A, which owns the SSE stream.

That requires shared state, sticky sessions, message routing, or another coordination system.

```text
Stateful MCP over HTTP
=
More features + more distributed systems complexity
```

```text
Stateless MCP over HTTP
=
Easier scaling + reduced protocol capability
```

---

## 24. `json_response=True`

The `json_response` flag disables streaming for POST responses.

Instead of receiving intermediate SSE messages during tool execution, the client receives only the final result as plain JSON.

With streaming:

```text
Tool Starts
→ Log Message
→ Progress Update
→ Final Result
```

With `json_response=True`:

```text
Tool Starts
→ Silence
→ Final JSON Result
```

Use JSON response mode when:

- streaming is unnecessary.
- plain JSON integration is preferred.
- the receiving system expects simple non-streaming HTTP responses.
- intermediate progress/logs are not required.

---

## 25. Transport Determines Capability

The biggest engineering lesson is:

```text
MCP Capability
=
Protocol Feature ∩ Transport Support ∩ Deployment Topology
```

It is not enough to say that MCP supports sampling, roots, logs, progress, or notifications.

The actual behavior depends on:

- transport type.
- stateful versus stateless mode.
- SSE availability.
- session handling.
- load balancer routing.
- streaming support.
- client callback implementation.

Architecture decides which features survive deployment.

---

# Course-Specific Sections

## Sampling as Server-Initiated Reasoning

Sampling makes MCP more than a tool execution protocol.

It allows the server to say:

```text
I have gathered data.
Now I need the client's model to reason over it.
```

This creates a feedback loop:

```text
Client
→ Server Tool
→ Client Model Call
→ Server Response
→ Client Final Output
```

This is useful, but it requires a transport that supports server-to-client requests.

Sampling works naturally with stdio and stateful StreamableHTTP.

It does not work when `stateless_http=True` removes server-to-client communication.

---

## Notifications as Execution Observability

Progress and logging notifications convert a long-running MCP tool from a black box into an observable process.

```text
Black Box Tool
Input → Unknown Processing → Output
```

```text
Observable Tool
Input → Phase Logs → Progress Updates → Output
```

This is important for UX, but it is also important for debugging.

A stuck tool is easier to diagnose if the last emitted state was:

```text
Parsed 18 / 100 files
```

instead of no signal at all.

---

## Roots as Filesystem Workspaces

Roots are not only file permissions.

They are scoped operating workspaces.

```text
Root
=
Authorized Filesystem Workspace
```

They improve both security and usability.

Security:

```text
Server cannot access outside approved roots.
```

Usability:

```text
User can say video.mp4 instead of typing full path.
```

Good file-based MCP servers should validate every file operation against roots.

---

## JSON Messages as Protocol Packets

MCP JSON messages are the protocol packets of the system.

Request-result messages are command-response packets.

Notifications are telemetry packets.

```text
Request-Result
=
Service Call
```

```text
Notification
=
Event / Telemetry
```

This distinction becomes essential when analyzing transport limitations.

A simple request-result pattern can survive stateless HTTP.

Server-initiated requests and notifications require richer transport support.

---

## Stdio as Full-Duplex Local Control

Stdio is the cleanest development transport because both sides can write messages whenever needed.

It is local-only, but behaviorally ideal.

```text
stdio
=
full local duplex pipe
```

It is the best starting point for learning and testing full MCP behavior.

---

## StreamableHTTP as Remote MCP with Trade-Offs

StreamableHTTP allows remote MCP servers, but it must work around HTTP's natural client-server directionality.

It uses SSE to create a server-to-client channel.

This enables powerful remote MCP behavior, but at the cost of added session and connection complexity.

```text
Remote MCP
=
HTTP deployment + SSE workaround + session management
```

---

## Stateless HTTP as Reduced MCP

Stateless HTTP is not simply a performance option.

It fundamentally changes what the MCP server can do.

```text
stateless_http=True
=
Easier scaling
+
Reduced bidirectional capability
```

This removes many advanced features:

- sampling.
- progress updates.
- subscriptions.
- server-to-client requests.
- session-aware behavior.

It is useful, but it is not equivalent to full MCP behavior.

---

# Force Agentic Command Lab Translation

The course can be summarized as:

```text
MCP Advanced
=
Operational Infrastructure For Agentic Tool Systems
```

The first MCP course established:

```text
Tools
→ Actions

Resources
→ Data

Prompts
→ Instructions
```

This advanced course adds:

```text
Sampling
→ Delegated reasoning

Notifications
→ Execution telemetry

Roots
→ Scoped operating surface

JSON Messages
→ Protocol packets

Transports
→ Communication topology

State
→ Session continuity

SSE
→ Remote server-to-client channel
```

For the **Force Agentic Command Lab**, these concepts translate into reusable command-system design rules.

---

## 1. Do Not Hardwire Intelligence Into Every Tool

Sampling shows that tools do not need to own model access.

A good command architecture separates:

```text
Tool
→ gather / compute / act

Model
→ interpret / summarize / decide

Client
→ permission / context / cost boundary

Server
→ reusable external capability
```

Command-lab principle:

```text
Let tools produce structured observations.
Let the assistant reason over them when needed.
```

---

## 2. Long-Running Commands Must Expose State

Notifications show that agentic systems should not disappear during long operations.

A good command should expose:

- current phase.
- completed work.
- remaining work.
- intermediate warnings.
- final validation.

Command-lab structure:

```text
Goal
→ Step Execution
→ Progress Update
→ Intermediate Validation
→ Final Artifact
```

Invisible execution reduces trust.

Observable execution creates auditable trust.

---

## 3. Define the Allowed Operating Surface

Roots map directly to command boundaries.

Every serious command should specify:

```text
Allowed Context
Forbidden Context
Writable Paths
Read-Only Paths
Validation Rules
```

Example:

```text
Goal:
  Generate documentation.

Allowed Context:
  ./README.md
  ./docs/
  ./src/

Forbidden Context:
  credentials
  unrelated projects
  private files

Validation:
  Do not edit outside approved paths.
```

This prevents vague prompts from becoming unbounded operations.

---

## 4. Match Workflow Design to Transport Capability

The course makes one architecture rule unavoidable:

```text
Do not design a workflow that depends on callbacks, sampling, or live progress if the transport cannot support them.
```

If the workflow requires:

```text
sampling
progress updates
logging notifications
resource subscriptions
roots requests
server-to-client messages
```

then it needs:

```text
stdio
or
stateful StreamableHTTP with SSE
```

If the workflow only requires:

```text
simple request-result tool calls
```

then stateless HTTP may be enough.

---

## 5. Architecture Eats Feature Lists

An MCP server may advertise advanced features, but deployment determines whether they work.

The command-lab version:

```text
Workflow Reliability
=
Prompt Quality
∩ Tool Design
∩ Transport Capability
∩ State Management
∩ Validation
```

This is the practical bridge from AI-course concept to engineering system.

---

# Relationship to the Markovian Unit-Step Protocol

MCP Advanced maps cleanly into the Markovian Unit-Step Protocol.

MUSP is:

```text
Observe
→ Act
→ Verify
→ Update State
→ Repeat
```

The advanced MCP features strengthen different parts of that loop.

---

## Sampling in MUSP

Sampling inserts client-owned reasoning into the loop.

```text
Observe:
  Server gathers data.

Act:
  Server creates a sampling request.

Verify:
  Client/model generates text.

Update State:
  Server incorporates generated result.

Repeat:
  Continue tool/model loop if needed.
```

State equation view:

```text
O_t = external data gathered by server
A_t = create_message(prompt)
V_t = generated model response
S_t+1 = enriched server state or final result
```

Sampling is delegated inference inside the Markovian loop.

---

## Notifications in MUSP

Notifications expose intermediate state.

```text
Observe:
  Tool receives input.

Act:
  Tool executes a step.

Verify:
  Tool emits log/progress signal.

Update State:
  Client/user now knows current execution phase.

Repeat:
  Continue until final result.
```

Mathematically:

```text
S_t     = current execution state
A_t     = current tool action
y_t     = emitted log/progress signal
S_t+1   = updated visible task state
```

Where:

```text
y_t = h(S_t, A_t)
```

The emitted message is a partial observation of the hidden execution state.

---

## Roots in MUSP

Roots act as constraints on available actions.

```text
Observe:
  User requests a file operation.

Act:
  System searches approved roots.

Verify:
  Check resolved path is inside an approved root.

Update State:
  Convert vague filename into authorized full path.

Repeat:
  Perform operation or request additional root access.
```

Constraint view:

```text
A_t is valid only if path ∈ R_allowed
```

Control analogy:

```text
u_t allowed iff x_target ∈ X_safe
```

Filesystem version:

```text
file_operation allowed iff path ∈ approved_root_set
```

---

## Transports in MUSP

Transport choice determines the richness of the loop.

With stdio or stateful StreamableHTTP:

```text
Observe
→ Act
→ Intermediate Verify
→ Update Session State
→ Repeat
```

With stateless HTTP or JSON response mode:

```text
Observe
→ Act
→ Final Result Only
```

Stateful transport supports closed-loop execution.

Stateless transport supports simpler request-response execution.

```text
stateful transport
=
closed-loop observable execution
```

```text
stateless transport
=
mostly open-loop request-response
```

MUSP benefits from stateful and observable channels because it depends on verification between steps.

---

# Personal Additions

## 1. MCP Servers as ROS Drivers for AI Tools

The robotics analogy becomes stronger in this course.

```text
MCP Server
→ ROS Driver For AI-Accessible Capability
```

A ROS driver exposes hardware through a standard interface.

An MCP server exposes tools, resources, prompts, filesystem access, and model-assisted workflows through a standard interface.

---

## 2. Sampling as Planner Delegation

Sampling is similar to a sensor-processing node asking the central planner for interpretation.

```text
Sensor Node
→ gathers raw data
→ asks planner for decision
→ receives high-level output
```

MCP version:

```text
Server
→ gathers external data
→ asks client/model for generation
→ receives synthesized text
```

The server has capability.

The client has cognition.

Sampling connects them.

---

## 3. Logs and Progress as Mission Telemetry

A rover does not silently disappear during a terrain survey.

It reports:

```text
Scanning terrain...
Mapping 35% complete...
Obstacle detected...
Replanning path...
Goal reached.
```

MCP tools should do the same for long-running operations.

```text
Fetching documents...
Processing sources...
Generating summary...
Validating schema...
Done.
```

Telemetry is not decoration.

It is an observability layer.

---

## 4. Roots as Safe Workspaces

Roots are the filesystem equivalent of a robot workspace envelope.

```text
Robot Arm
→ allowed workspace

MCP File Server
→ approved filesystem roots
```

A robot should not move outside its safe workspace.

An MCP server should not read or write outside approved roots.

---

## 5. Transport as Communication Topology

MCP message design is only half the system.

The transport determines what interaction loops are physically possible.

Robotics analogy:

```text
MCP JSON Messages
→ command / telemetry packet format

Transport
→ radio, Ethernet, CAN, serial, or network link

Session ID
→ node / vehicle identity

SSE Stream
→ telemetry downlink

POST Request
→ command uplink
```

Stdio is like a direct serial cable.

StreamableHTTP is like remote command-and-control over a network.

Stateless HTTP is like a simple command endpoint with no live mission telemetry.

---

## 6. Production MCP Is Distributed Systems Engineering

The advanced course reveals a serious engineering truth:

```text
MCP features are constrained by deployment architecture.
```

Before designing an MCP server, ask:

- Does it need sampling?
- Does it need roots?
- Does it need progress updates?
- Does it need logging notifications?
- Will it run locally or remotely?
- Will it sit behind a load balancer?
- Does it need horizontal scaling?
- Can the transport support server-to-client messages?
- Is streaming allowed?
- Is session continuity required?

These questions decide the architecture.

---

# Overall Assessment

## Strengths

- Strong explanation of sampling as client-owned model access.
- Clear treatment of why public MCP servers should avoid owning model costs.
- Useful explanation of logs and progress notifications as UX and observability improvements.
- Practical coverage of `Context`, `context.info()`, and `context.report_progress()`.
- Strong introduction to roots as both file discovery and access control.
- Good explanation of JSON request-result versus notification patterns.
- Clear explanation of the MCP initialization sequence.
- Useful stdio transport model for local development and testing.
- Strong explanation of StreamableHTTP's limitations and SSE workaround.
- Valuable discussion of `stateless_http` and `json_response` trade-offs.
- Good bridge from basic MCP concepts to production deployment concerns.
- Strong relevance to real agentic infrastructure design.

## Weaknesses

- Security discussion around roots is conceptually useful but not deeply formalized.
- Path traversal, symlink handling, and filesystem race conditions deserve deeper treatment.
- Authentication and authorization for remote MCP servers are not covered in depth.
- StreamableHTTP scaling could use more diagrams around sticky sessions and shared state.
- Deployment patterns with real load balancers are only introduced conceptually.
- Failure modes for SSE disconnections, retries, and partial streams are not deeply explored.
- Sampling safety, model selection, and prompt-injection risks are not deeply covered.
- The course gives the architecture but not a full production hardening checklist.

---

# Quiz Results

The final assessment contained ten questions.

Results:

```text
Correct Answers: 10 / 10
Score: 100%
Elapsed Time: 9 minutes
```

Topics tested:

- using sampling when the server needs Claude without owning API costs.
- identifying request-result message patterns.
- understanding StreamableHTTP's SSE workaround.
- using roots for filesystem discovery and access control.
- enabling `json_response=True` for plain JSON responses.
- selecting stdio transport for local same-machine testing.
- identifying stdio as requiring client and server on the same machine.
- defining roots in MCP.
- MCP initialization sequence.
- defining sampling in MCP.

---

# Final Takeaway

Model Context Protocol: Advanced Topics shows that MCP is not only about exposing tools, resources, and prompts.

It is about designing reliable communication infrastructure for agentic systems.

The central operating principle is:

```text
Advanced MCP behavior depends on bidirectional communication, scoped context, observable execution, and transport-aware deployment.
```

The course extends the earlier MCP model:

```text
Tools
→ Actions

Resources
→ Data

Prompts
→ Instructions
```

with the advanced infrastructure layer:

```text
Sampling
→ Server-initiated, client-owned model inference

Logging / Progress
→ Execution telemetry

Roots
→ Authorized filesystem workspaces

JSON Message Types
→ Protocol packet structure

Stdio
→ Full local bidirectional transport

StreamableHTTP
→ Remote MCP with SSE-based bidirectional workaround

stateless_http
→ Easier scaling with reduced MCP capability

json_response
→ Simpler plain JSON responses without streaming
```

The strongest engineering lesson is:

```text
Protocol features do not exist in isolation.
They only work when the transport and deployment topology support their required message flow.
```

In one sentence:

```text
MCP Advanced turns MCP from an integration interface into a distributed agent infrastructure problem.
```

---