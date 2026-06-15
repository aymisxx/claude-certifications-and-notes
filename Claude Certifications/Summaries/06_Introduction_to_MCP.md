# Introduction to Model Context Protocol Completion Notes

**Date:** 2026-06-15
**Course:** Anthropic SkillJar - Introduction to Model Context Protocol
**Status:** Completed
**Quiz Score:** 7/7 - 100%
**Quiz Time:** 3 minutes

# Overview

Today I completed the Anthropic SkillJar **Introduction to Model Context Protocol** course as part of the broader objective of extracting reusable agentic infrastructure concepts for the **Force Agentic Command Lab** repository.

The course focused on **Model Context Protocol (MCP)** as a standardized communication layer that connects AI applications to external tools, resources, and reusable prompts without requiring every application developer to manually implement large custom integrations.

The primary value of the course was not Claude-specific onboarding, but the broader engineering principles behind:

- standardized tool integration.
- client-server agent architecture.
- transport-agnostic communication.
- tool discovery and execution.
- reusable integration servers.
- MCP client implementation.
- MCP server implementation.
- Python SDK-based tool definition.
- resource exposure.
- prompt templates.
- inspector-based debugging.
- separation of reasoning, communication, integration, and data layers.

---

# Curriculum Covered

## Introducing MCP

- Model Context Protocol fundamentals.
- MCP clients and MCP servers.
- External service integration.
- GitHub integration example.
- MCP versus direct API calling.
- MCP versus tool use.
- Tools, prompts, and resources.

## MCP Clients

- Client-server communication.
- Transport-agnostic communication.
- Standard input/output transport.
- HTTP and WebSocket transports.
- ListToolsRequest.
- ListToolsResult.
- CallToolRequest.
- CallToolResult.
- Full user-query-to-tool-result flow.

## Defining Tools with MCP

- Python MCP SDK.
- FastMCP.
- Tool decorators.
- Pydantic Field descriptions.
- Automatic schema generation.
- Document reader tool.
- Document editor tool.
- Python exception handling.
- Tool registration.

## The Server Inspector

- MCP Inspector.
- Local development testing.
- Browser-based server inspection.
- Tool testing before application integration.
- `mcp dev` workflow.

## Implementing a Client

- MCP Client class.
- Client Session.
- Session resource management.
- list_tools.
- call_tool.
- End-to-end CLI application flow.

## Defining Resources

- Resources as read-only data interfaces.
- Direct resources.
- Templated resources.
- URI-based resource access.
- MIME types.
- JSON, text, and binary resources.
- Resource testing in the inspector.

## Accessing Resources

- read_resource implementation.
- AnyUrl.
- ReadResourceRequest.
- ReadResourceResult.
- Content parsing.
- MIME-type-based response handling.
- Resource injection into prompts.

## Defining Prompts

- MCP prompts as reusable message templates.
- Prompt decorators.
- Parameterized prompt functions.
- Returning message lists.
- Document formatting prompt example.
- Prompt testing in the inspector.

## Prompts in the Client

- list_prompts.
- get_prompt.
- Prompt arguments.
- Slash-command style UX.
- Prompt retrieval and interpolation.

---

# Key Concepts Learned

## 1. MCP as a Communication Layer

Model Context Protocol is a standardized communication layer between AI applications and external systems.

```text
AI Application
↔
MCP Client
↔
MCP Server
↔
External Service
```

The purpose is to avoid repeatedly writing large custom integrations for every external system.

Instead of every application manually defining hundreds of tools for services like GitHub, an MCP server can expose those capabilities in a standardized way.

---

## 2. The Problem MCP Solves

Without MCP, an application developer must define and maintain:

- tool schemas.
- function implementations.
- API calls.
- authentication handling.
- validation.
- error handling.
- integration updates.

For a large service like GitHub, this can become a major maintenance burden.

```text
Without MCP
=
Application Developer Maintains Integration
```

With MCP:

```text
Application
→ MCP Client
→ MCP Server
→ External API
```

The MCP server handles the service-specific implementation.

```text
With MCP
=
Application Connects To Reusable Integration
```

---

## 3. MCP Servers

An MCP server provides standardized access to outside data and functionality.

It can expose:

```text
Tools
+
Resources
+
Prompts
```

A GitHub MCP server might expose operations such as:

```text
get_repos
get_pull_requests
get_issues
search_code
create_issue
```

The application does not need to know the internal details of the GitHub API.

The MCP server acts as an adapter layer.

---

## 4. MCP Is Not the Same as Tool Use

MCP and tool use are complementary but different concepts.

```text
Tool Use
=
Claude Calls A Tool
```

```text
MCP
=
A Standard Way To Provide Tools, Resources, And Prompts
```

Tool use is about how the model invokes a capability.

MCP is about where that capability comes from and how it is exposed to the application.

---

## 5. MCP Clients

The MCP client is the communication bridge between an application and one or more MCP servers.

```text
Application
→ MCP Client
→ MCP Server
```

The client handles:

- message exchange.
- protocol details.
- transport communication.
- tool discovery.
- tool execution requests.
- resource access.
- prompt retrieval.

The application can focus on its own logic rather than low-level MCP protocol handling.

---

## 6. Transport-Agnostic Communication

MCP is transport agnostic.

This means the protocol can operate over different communication mechanisms.

Examples:

```text
stdin / stdout

HTTP

WebSockets
```

The transport can change while the protocol semantics remain stable.

This separates communication method from application logic.

---

## 7. Tool Discovery

MCP clients discover available tools through:

```text
ListToolsRequest
→
ListToolsResult
```

The client asks the server:

```text
What tools do you provide?
```

The server responds with tool definitions.

This allows an application to send Claude the currently available tool set.

---

## 8. Tool Execution

MCP clients execute tools through:

```text
CallToolRequest
→
CallToolResult
```

The client requests execution of a specific tool with arguments.

The MCP server performs the actual tool execution.

```text
MCP Client
=
Requests Execution

MCP Server
=
Executes Tool
```

This distinction is foundational.

---

## 9. Full MCP Tool Flow

A user asks:

```text
What repositories do I have?
```

The full flow is:

```text
User Query
→ Application Server
→ MCP Client Lists Tools
→ MCP Server Returns Tools
→ Application Sends Query + Tools To Claude
→ Claude Chooses Tool
→ Application Requests Tool Execution Through MCP Client
→ MCP Server Calls External API
→ External API Returns Data
→ MCP Server Returns Tool Result
→ Application Sends Tool Result To Claude
→ Claude Produces Final Answer
→ User Receives Response
```

This may involve many steps, but each layer has a clear responsibility.

---

# Defining Tools With MCP

## FastMCP

The course used the Python MCP SDK and FastMCP.

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("DocumentMCP", log_level="ERROR")
```

This initializes an MCP server instance.

---

## Tool Decorators

The SDK allows tools to be defined with decorators.

```python
@mcp.tool(
    name="read_doc_contents",
    description="Read the contents of a document and return it as a string."
)
def read_document(
    doc_id: str = Field(description="Id of the document to read")
):
    ...
```

The decorator registers the function as an MCP tool.

The SDK automatically handles the tool schema.

---

## Pydantic Field Descriptions

Function parameters use type hints and Pydantic Field descriptions.

```python
doc_id: str = Field(description="Id of the document to read")
```

This gives Claude better information about expected inputs.

The SDK uses the function signature and metadata to generate the tool schema.

---

## Tool Implementation

The example document reader tool follows:

```text
Input
→ doc_id

Operation
→ Lookup Document

Output
→ Document Contents
```

The document editor tool follows:

```text
Input
→ doc_id, old_str, new_str

Operation
→ Replace Text

Output
→ Updated Document State
```

---

## Error Handling

The course used normal Python exceptions.

```python
raise ValueError(f"Doc with id {doc_id} not found")
```

This allows MCP tool failure to be handled naturally through Python error mechanisms.

---

# MCP Inspector

The MCP Inspector provides a browser-based way to test and debug MCP servers.

Launch command:

```bash
mcp dev mcp_server.py
```

The inspector allows developers to test:

- available tools.
- tool parameters.
- tool responses.
- resource outputs.
- prompt templates.
- error behavior.

This is useful before connecting the MCP server to a complete application.

```text
MCP Inspector
=
Development Test Harness
```

---

# Implementing an MCP Client

## Client Components

The MCP client implementation contains two main parts:

```text
MCP Client Class
+
Client Session
```

The client session is the actual SDK connection to the MCP server.

The MCP Client class wraps that session and makes it easier to use safely.

This wrapper helps manage:

- connections.
- cleanup.
- session access.
- application-level methods.

---

## list_tools

The `list_tools` method retrieves available tools from the server.

```python
async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools()
    return result.tools
```

This maps directly to tool discovery.

---

## call_tool

The `call_tool` method executes a selected tool.

```python
async def call_tool(
    self, tool_name: str, tool_input: dict
) -> types.CallToolResult | None:
    return await self.session().call_tool(tool_name, tool_input)
```

This maps directly to tool execution.

---

## End-to-End Client Flow

A complete client-enabled application can:

```text
List Tools
→ Send Tools To Claude
→ Receive Tool Use Request
→ Call Tool Through MCP Client
→ Return Result To Claude
→ Produce Final Response
```

The client bridges Claude-facing application logic and MCP server functionality.

---

# Resources

## What Resources Are

Resources expose data to MCP clients.

They are similar to GET handlers in HTTP servers.

```text
Resource
=
Read-Only Data Interface
```

Resources are useful when information should be fetched and included directly in the prompt rather than retrieved through a model-driven tool call.

---

## Direct Resources

Direct resources have static URIs.

Example:

```python
@mcp.resource(
    "docs://documents",
    mime_type="application/json"
)
def list_docs() -> list[str]:
    return list(docs.keys())
```

This is useful for operations that do not require parameters.

```text
Direct Resource
=
Static URI
```

---

## Templated Resources

Templated resources contain parameters in their URIs.

Example:

```python
@mcp.resource(
    "docs://documents/{doc_id}",
    mime_type="text/plain"
)
def fetch_doc(doc_id: str) -> str:
    ...
```

The SDK parses the URI and passes the parameter into the function.

```text
Templated Resource
=
URI With Parameters
```

---

## MIME Types

Resources specify a MIME type to help clients parse returned data.

Examples:

```text
application/json
text/plain
application/pdf
```

The client can use this to decide whether to parse JSON, display plain text, or handle binary data.

---

## Resource Protocol

Resource access follows:

```text
ReadResourceRequest
→
ReadResourceResult
```

The client sends a URI.

The server resolves the URI and returns content.

---

# Accessing Resources

The client can expose a `read_resource` method.

```python
async def read_resource(self, uri: str) -> Any:
    result = await self.session().read_resource(AnyUrl(uri))
    resource = result.contents[0]

    if isinstance(resource, types.TextResourceContents):
        if resource.mimeType == "application/json":
            return json.loads(resource.text)

    return resource.text
```

Important concepts:

- URI identifies the resource.
- the result contains a list of contents.
- MIME type controls parsing.
- JSON content can be decoded automatically.
- plain text can be returned directly.

---

## Resource Injection

Resources can be used for mention-style workflows.

Example:

```text
@report.pdf
```

The system can fetch the resource and inject its content into the prompt.

This avoids a separate model tool call.

```text
Resource
→ Direct Context Injection
```

---

# Prompts

## What Prompts Are

MCP prompts are reusable, server-defined message templates.

```text
Prompt
=
Reusable Instruction Template
```

They allow server authors to provide high-quality, tested prompts for common workflows.

---

## Why Prompts Matter

Users can write their own instructions, but server-defined prompts can be more consistent and reliable.

A document MCP server might expose prompts for:

- formatting documents.
- summarizing documents.
- analyzing documents.
- generating reports.

The server author can encode domain-specific best practices into the prompt.

---

## Defining Prompts

Prompts are defined with decorators.

```python
@mcp.prompt(
    name="format",
    description="Rewrites the contents of the document in Markdown format."
)
def format_document(
    doc_id: str = Field(description="Id of the document to format")
) -> list[base.Message]:
    ...
```

The function returns a list of messages that can be sent to Claude.

---

## Prompt Arguments

Prompts can accept parameters.

Example:

```python
{"doc_id": "plan.md"}
```

The MCP server interpolates these arguments into the prompt function.

---

## Prompts in the Client

The client implements:

```python
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts
```

and:

```python
async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages
```

This allows applications to list available prompts and retrieve parameterized prompt messages.

---

## Slash-Command UX

Prompts can appear as slash commands.

Example:

```text
/format plan.md
```

The client retrieves the prompt, fills in arguments, and sends the resulting messages to Claude.

---

# Tools vs Resources vs Prompts

The course establishes three MCP primitives.

```text
Tools
→ Actions

Resources
→ Data

Prompts
→ Instructions
```

This is the most important conceptual model in the course.

Tools let Claude do things.

Resources provide data.

Prompts provide reusable workflows and instructions.

---

# Force Agentic Command Lab Translation

The course can be summarized as:

```text
Model Context Protocol
=
Standardized Agent Integration Layer
```

More specifically:

```text
MCP
=
Tools
+
Resources
+
Prompts
+
Transport
+
Client-Server Protocol
```

Potential future examples for robotics and autonomy workflows include:

```text
Telemetry MCP

Fleet MCP

Mission MCP

Simulation MCP

Map MCP

Repository MCP
```

Each MCP server could expose:

```text
Tools
→ Run simulation, query telemetry, dispatch robot

Resources
→ Maps, mission logs, fleet state, configuration files

Prompts
→ Generate mission summary, format failure report, plan survey route
```

---

# Relationship to the Markovian Unit-Step Protocol

MCP fits naturally into the Markovian Unit-Step Protocol.

A tool call becomes an action:

```text
aₜ = CallTool(tool_name, arguments)
```

The MCP server returns an observation:

```text
oₜ₊₁ = CallToolResult
```

The model updates state based on the result:

```text
sₜ₊₁ = Update(sₜ, oₜ₊₁)
```

Resources provide state information directly:

```text
Resource URI
→ Context Observation
```

Prompts provide reusable transition policies:

```text
Prompt Template
→ Structured Instruction Sequence
```

MCP therefore supplies standardized inputs, actions, and reusable procedures for agentic loops.

---

# Personal Additions

## 1. MCP as ROS Drivers for AI

A useful robotics analogy is:

```text
MCP Server
→ ROS Driver For AI Tools
```

Just as ROS drivers expose hardware through standard interfaces, MCP servers expose external services through standardized AI-accessible interfaces.

---

## 2. Separation of Concerns

MCP separates:

```text
Reasoning
→ Claude

Communication
→ MCP Client

Tool Implementation
→ MCP Server

Data
→ External Service
```

This makes agent systems easier to reason about and maintain.

---

## 3. Integration Reuse

The major value of MCP is integration reuse.

```text
Build Once
→ Expose Through MCP
→ Reuse Across Clients
```

This is especially important for large services with many capabilities.

---

## 4. MCP as Agent Infrastructure

Skills provide reusable knowledge.

Subagents provide reusable workers.

MCP provides reusable external capabilities.

Together:

```text
Skills
+
Subagents
+
MCP
=
Reusable Agent Architecture
```

---

# Overall Assessment

## Strengths

- Clear introduction to MCP architecture.
- Strong explanation of client-server responsibilities.
- Good practical example using document management.
- Useful distinction between MCP and tool use.
- Good treatment of tool discovery and execution.
- Practical Python SDK workflow.
- Strong explanation of resources and prompts.
- MCP Inspector provides a valuable debugging workflow.
- Strong relevance to agentic software architecture.

## Weaknesses

- The course is introductory and does not deeply cover security.
- Authentication and authorization are only lightly discussed.
- Production deployment concerns are not deeply explored.
- Transport details are simplified.
- Error handling is introduced but not deeply formalized.
- Advanced orchestration and multi-server patterns are outside the scope.

---

# Quiz Results

The final assessment contained seven questions.

Results:

```text
Correct Answers: 7 / 7
Score: 100%
Elapsed Time: 3 minutes
```

Topics tested:

- MCP client components.
- tool discovery message types.
- MCP Inspector usage.
- the GitHub integration problem MCP solves.
- selecting prompts for user-triggered workflows.
- defining tools with the Python MCP SDK.
- choosing templated resources for parameterized URIs.

---

# Final Takeaway

Introduction to Model Context Protocol shows that modern agent systems require more than prompts, memories, and workers.

They need a standardized way to access external capabilities.

MCP provides that layer.

The central operating principle is:

```text
Do not rebuild every integration inside every application.
```

Instead:

```text
Expose capabilities through MCP servers.

Discover them through MCP clients.

Let models use them through standardized tool, resource, and prompt interfaces.
```

More broadly:

```text
Tools
→ Actions

Resources
→ Data

Prompts
→ Instructions
```

The course reinforces a powerful engineering idea:

```text
Standardize integration.

Separate responsibilities.

Reuse capabilities.

Let agents operate through clean interfaces.
```

---