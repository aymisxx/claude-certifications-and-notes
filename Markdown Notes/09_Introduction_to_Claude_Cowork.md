# Introduction to Claude Cowork Completion Notes

**Date:** 2026-06-15
**Course:** Anthropic SkillJar - Introduction to Claude Cowork
**Status:** Completed
**Quiz Score:** 8/8 - 100%
**Quiz Time:** 2 minutes

# Overview

Today I completed the Anthropic SkillJar **Introduction to Claude Cowork** course as part of the broader objective of building a structured archive of Claude certifications, course notes, and reusable agentic workflow translations.

The course introduced **Claude Cowork** as a different way of working with Claude.

The central idea is that Cowork is not just a more capable chat window.

It is a delegated-work environment where Claude can operate on local files, connected applications, browser surfaces, and real deliverables while keeping the user in control through planning, approvals, steering, and review.

The course focused on:

- what Cowork is.
- how Cowork differs from Chat and Code.
- how to set up folders, connectors, and permissions.
- what kinds of tasks fit Cowork.
- how to delegate a first real task.
- how global instructions, projects, skills, and plugins make Cowork better over time.
- how Claude in Chrome and Claude for Microsoft 365 extend Cowork into browser and Office workflows.
- how to work safely with autonomous file and tool access.
- how to validate skills before sharing them.
- how plugins become team-level workflow infrastructure.

The main lesson is simple:

```text
Cowork is for delegating real work, not just asking for answers.
```

---

# Curriculum Covered

## What Is Claude Cowork

- Cowork as a mode in Claude Desktop.
- Cowork as delegated work rather than turn-by-turn chat.
- Claude working on local files, connected apps, browser pages, and tools.
- Cowork producing finished artifacts.
- Difference between Chat, Cowork, and Code.
- Anatomy of a good Cowork task.

## Setting Up Claude Cowork

- Installing and opening Cowork in the Claude Desktop app.
- Pointing Claude at a working folder.
- Choosing a scoped folder rather than a broad catch-all directory.
- Understanding folder read/write permissions.
- Adding connectors for email, calendar, messaging, cloud storage, CRM, and project tools.
- Understanding Claude in Chrome for sites without connectors.
- Using permission modes: Ask before acting and Act without asking.
- Permanent file deletion always requiring confirmation.

## What Claude Cowork Can Do

- Three major Cowork task patterns.
- Multi-step tasks.
- File-based tasks.
- Multi-tool tasks.
- Scheduled tasks through `/schedule`.
- Dispatch from mobile to desktop Cowork.
- Computer-on and Claude-running requirements for scheduled tasks and Dispatch.

## Hand Claude Cowork Your First Task

- Delegating a real task end-to-end.
- Naming the deliverable.
- Naming the inputs.
- Naming task nuance and constraints.
- Answering clarifying questions.
- Steering mid-task.
- Reviewing finished deliverables.
- Treating Cowork output as a draft from a capable colleague that still requires review.

## Get Better Results Faster

- Four compounding building blocks.
- Global instructions.
- Projects.
- Skills.
- Plugins.
- Reusable context and process packaging.

## Global Instructions and Projects

- Global instructions as a standing brief.
- Project-scoped instructions.
- Project folders and context links.
- Project scheduled tasks.
- Project memory.
- Three ways to start a project.

## Skills

- Skills as reusable playbooks.
- `SKILL.md` as the instruction file.
- Skill assets.
- Skill references.
- Skill scripts.
- Building and updating skills with Claude.
- Automatic skill use when tasks match.

## Plugins

- Plugins as packaged sets of skills, connectors, and subagents.
- End-to-end process plugins.
- Team toolkit plugins.
- Marketplace plugins.
- Customizing plugins to match team work.
- Building plugins from mature skills.

## Claude in Chrome

- Claude in Chrome as the bridge for web tools without connectors.
- Reading, clicking, navigating, and extracting information from browser pages.
- Working with internal dashboards, portals, BI tools, and web apps behind login.
- Combining Chrome work with Cowork deliverables.
- Browser-access watch-outs.

## Claude for Microsoft 365

- Claude inside Word, Excel, PowerPoint, and Outlook.
- Editing and refining in the document itself.
- Carrying context across Office apps.
- Cowork versus M365 use cases.

## Working Safely

- Workspace scoping.
- Dedicated working folders.
- Backups for irreplaceable files.
- Testing workflows on copies.
- Specific prompts for destructive verbs.
- Bounded prompts.
- Draft-first scheduled tasks.
- Reading plans.
- Watching for unexpected patterns.
- Deliberate approvals.
- Knowing when Cowork is not the right tool.

## Validating Skills and Sharing Plugins

- Evals as lightweight try-outs.
- Comparing with-skill and without-skill outputs.
- Giving specific feedback.
- Iterating skills.
- Sharing plugins through an organization marketplace.
- Available, installed-by-default, and required plugin modes.
- Maintaining shared plugins over time.

---

# Key Concepts Learned

## 1. Cowork Is Delegation, Not Chat

The biggest mental model shift is that Cowork is not mainly for questions.

Chat is where the user thinks with Claude.

Cowork is where the user delegates a piece of work to Claude.

```text
Chat
→ Think with Claude

Cowork
→ Delegate work to Claude

Code
→ Build software with Claude
```

Chat is turn-by-turn.

Cowork sustains a whole piece of work across planning, file access, tool use, and artifact production.

Code works inside a repository with developer tools such as terminal, git, file editing, and test execution.

The practical difference:

```text
Chat:
  Help me think through this.

Cowork:
  Go do this piece of work and save the output.

Code:
  Modify this repository and verify the code.
```

---

## 2. Cowork Works in the User's Environment

Cowork works where the work actually lives.

It can work:

```text
on local files
in connected apps
inside browser workflows
with tools and connectors
```

The working folder is central.

When the user points Cowork at a folder, Claude can read, create, edit, and save files there directly.

This is different from Chat.

```text
Chat:
  Claude can read uploaded files.

Cowork:
  Claude can work directly inside a local folder.
```

This turns the folder into a bounded operating workspace.

---

## 3. The Working Folder Is a Boundary

The folder determines what local files are in scope.

Best practice:

```text
Pick the smallest folder that contains the task context.
```

Bad setup:

```text
~/Documents
~/Downloads
~/Desktop
```

Better setup:

```text
~/Work/Q3_Client_Memo/
~/Projects/Northwind_QBR/
~/Reports/Monthly_Metrics_Copy/
```

The working folder provides both capability and risk boundary.

```text
Cowork folder
=
authorized workspace boundary
```

This is conceptually similar to MCP roots.

---

## 4. Connectors Expand Cowork's Reach

Connectors allow Claude to access work applications.

Common connectors include:

```text
Email and calendar:
  Gmail, Outlook, Microsoft 365

Messaging:
  Slack, Teams

Cloud storage:
  Google Drive, OneDrive, SharePoint, Box

CRM and project tools:
  Salesforce, HubSpot, Notion, Asana, Linear
```

Once connected, the user can reference these sources naturally.

Examples:

```text
Check what the team said in Slack about the launch.

Find the customer follow-up email from last quarter.

Use the planning doc in Drive.
```

Connectors make Cowork a cross-application work agent rather than a folder-only assistant.

---

## 5. Permissions Keep Delegation Bounded

Cowork has permission controls because it can take real actions.

The default mode is:

```text
Ask before acting
```

Claude pauses before actions such as:

```text
sending email
posting messages
sharing files
taking outside-world actions
```

The faster but riskier mode is:

```text
Act without asking
```

This should only be used for trusted tools, low-risk tasks, and actively supervised workflows.

One rule always remains:

```text
Claude always asks before permanently deleting files.
```

No exception.

---

## 6. Good Cowork Tasks Have a Shape

Cowork is best for tasks that are:

```text
multi-step
file-based
multi-tool
artifact-producing
reviewable
```

Examples:

```text
Read five vendor PDFs, compare price and SLAs, and create a spreadsheet.

Turn a folder of meeting notes and spreadsheets into a finished presentation saved to the folder.

Search Slack, email, Drive, and meeting notes to create an executive summary.
```

Poor Cowork fits are usually better for Chat:

```text
rewrite a pasted paragraph
explain a concept quickly
brainstorm names turn by turn
answer one bounded question
```

The selection rule:

```text
If the value is in the doing, not just the thinking, consider Cowork.
```

---

## 7. A Good Cowork Prompt Names Deliverable, Inputs, and Nuance

A good Cowork prompt does three things.

```text
1. Names the deliverable.
2. Names the inputs.
3. Names the nuance.
```

## Deliverable

Examples:

```text
a one-page brief
a four-page memo
a slide for the QBR
a spreadsheet comparison
a ranked list with notes
```

## Inputs

Examples:

```text
Q3 Competitive Review folder
last quarter's memo
analyst-call PDFs
Slack launch channel
Gmail sales threads
Google Drive planning doc
```

## Nuance

Examples:

```text
for the executive team
lead with the recommendation
flag anything unverifiable
include base, best, and worst case scenarios
focus on pricing-tier decision
```

Prompt shape:

```text
Create [deliverable]
using [inputs]
with [nuance / constraints / audience / decision context].
```

---

## 8. Clarifying Questions Are Context-Gap Closure

Cowork often asks clarifying questions before starting non-trivial tasks.

This is not friction.

It is the system closing context gaps before execution.

```text
Ambiguity detected
→ Claude asks question
→ User resolves ambiguity
→ Claude starts with better state
```

Examples:

```text
Which memo should I use as the format reference?

Should I include all PDFs or only analyst-call PDFs?

Should the memo be written for executives or the sales team?
```

In Chat, ambiguity is often resolved through follow-up turns.

In Cowork, ambiguity should be resolved before the delegated execution begins.

---

## 9. Mid-Task Steering Is Closed-Loop Supervision

Cowork lets the user intervene while Claude is working.

The user should steer if Claude is drifting toward:

```text
wrong source
wrong format
wrong tone
wrong analysis direction
wrong deliverable structure
wrong external action
```

Key principle:

```text
Do not wait until the end if the plan is visibly wrong.
```

Cowork supports course correction during execution.

```text
plan executes
→ user observes progress
→ user injects correction
→ Claude updates trajectory
```

This is supervised autonomy.

---

## 10. Finished Cowork Output Still Requires Review

The finished deliverable is the artifact, not the chat.

The user should review it like work from a capable colleague whose judgment still needs checking.

Review checklist:

```text
Does it meet the actual objective?
Are facts and figures traceable to source files?
Did Claude identify the source documents?
Does anything sound fabricated?
Is the tone and format appropriate?
Are there hidden assumptions?
Should the draft be edited instead of regenerated?
```

A polished result can still be wrong.

The recommended habit is:

```text
Review it yourself before sending or acting on it.
```

---

## 11. Global Instructions Calibrate Cowork

Global instructions are a standing brief that applies across Cowork sessions.

They tell Claude:

```text
who the user is
what the user works on
what formats the user prefers
how output should be delivered
what assumptions should be flagged
what source standards matter
```

Useful global instruction content:

```text
role and responsibilities
recurring deliverables
preferred writing style
default output length
team acronyms
format preferences
review expectations
assumption-handling rules
citation/source rules
```

Design rule:

```text
Global instructions should describe stable preferences, not temporary state.
```

---

## 12. Projects Provide Scoped Workstream Context

Global instructions describe the user.

Projects describe a specific stream of work.

```text
Global instructions
→ me

Project
→ this account / deliverable / initiative
```

A project can include:

```text
project instructions
scheduled tasks
context folders or links
project memory
```

The key difference from generic Cowork sessions:

```text
Outside a project:
  session starts mostly fresh, plus global instructions

Inside a project:
  Claude accumulates memory from prior project conversations
```

Project memory stays inside that project.

Chat memory is broader and can apply across conversations.

---

## 13. Skills Teach Cowork a Reusable Process

A skill is a reusable playbook.

```text
Skill
=
folder of instructions, assets, references, and scripts
```

The core file is:

```text
SKILL.md
```

Claude loads the skill when a task matches it.

```text
Task matches skill
→ Claude loads skill
→ Claude follows procedure
→ Output follows team process
```

A skill can include:

```text
Instructions:
  SKILL.md process description

Assets:
  logos, templates, slide masters, fonts

References:
  examples, style guides, clause libraries, past work

Scripts:
  deterministic calculations, transformations, formatters
```

The rule is:

```text
Include what the process needs, nothing extra.
```

---

## 14. Plugins Bundle Skills Into Role or Team Expertise

A plugin is a packaged set of skills built around a job, role, or workflow.

```text
Skill
→ one reusable process

Plugin
→ bundled skills + connectors + subagents around a job
```

Plugins may include:

```text
multiple skills
connectors
subagents
role-specific workflows
team-specific expertise
```

Two common plugin shapes:

```text
End-to-end process plugin:
  several sequential skills for one workflow pipeline

Team toolkit plugin:
  several frequently used skills for one role or function
```

Example end-to-end process:

```text
Monthly close plugin
├── pull actuals skill
├── build variance table skill
├── draft board memo skill
└── package final report skill
```

Example team toolkit:

```text
Finance plugin
├── variance analysis
├── financial modeling
├── investment memo drafting
└── quarterly reports
```

---

## 15. Subagents Handle Independent Pieces With Focused Context

Subagents let Cowork split large work into independent parts.

Each subagent can work on a separate piece with its own focused context.

```text
Large task
├── research subagent
├── drafting subagent
├── review subagent
└── analysis subagent
```

This is useful when work can be decomposed into parallel or semi-independent components.

```text
subagent
=
specialist helper with isolated focus
```

---

## 16. Claude in Chrome Bridges Browser-Only Work

Claude in Chrome is for web tools without connectors.

It can:

```text
read browser pages
click
navigate
interact with web apps
extract data
hand results back to Cowork
```

Useful contexts:

```text
internal dashboards
vendor portals
customer systems
BI tools
procurement portals
web apps behind login
web research workflows
```

Pattern:

```text
Browser data
→ Chrome interaction
→ exported context
→ Cowork synthesis
→ saved deliverable
```

Watch-outs:

```text
Claude cannot sign in for the user.
The user must already be authenticated.
Claude sees what the user can see.
Sensitive websites need careful approval boundaries.
```

---

## 17. Claude for Microsoft 365 Works Inside Documents

Claude for Microsoft 365 appears inside:

```text
Word
Excel
PowerPoint
Outlook
```

This is different from Cowork.

```text
Cowork:
  pulls from many sources and builds deliverables

Claude inside M365:
  works inside the file or app currently open
```

Examples:

```text
Excel:
  analyze data, write formulas, debug #REF errors, create variance commentary

PowerPoint:
  build slides, match slide masters, create native editable charts

Word:
  draft, revise, reformat, work with comments and tracked changes

Outlook:
  triage mail and draft replies using thread and calendar context
```

The strongest pattern is cross-app context transfer.

```text
Outlook
→ Word
→ Excel
→ PowerPoint
→ Outlook
```

Most real work can use both Cowork and M365.

```text
Cowork builds the first draft.
M365 refines the file in place.
```

---

## 18. Scheduled Tasks Run Only When Desktop Cowork Can Run

Scheduled tasks are created with:

```text
/schedule
```

They can run on a cadence such as:

```text
hourly
daily
weekdays
manual
```

Important limitation:

```text
Scheduled tasks only run when the computer is on, awake, and Claude is running.
```

If the computer is asleep when the task is due:

```text
Cowork runs the task as soon as the user is back and reports that it was delayed.
```

Good scheduled tasks start in draft mode.

```text
Draft for review first.
Automate sending only after the workflow is trusted.
```

---

## 19. Dispatch Sends Tasks From Mobile to Desktop

Dispatch lets the user assign a Cowork task from the Claude mobile app.

Flow:

```text
User starts task on phone
→ task runs on desktop Cowork
→ same local files
→ same connectors
→ same permissions
→ user receives notification when done
```

Dispatch is mobile-triggered desktop delegation.

Limitations:

```text
desktop must be on
desktop must be awake
Claude must be signed in
Cowork must be open
availability may depend on plan or admin settings
```

---

## 20. Safety Requires Workspace, Prompt, and Approval Discipline

Cowork has real action surfaces.

That means safety requires active design.

The risk model:

```text
risk ≈ authority × scope × ambiguity × irreversibility
```

Reduce risk by reducing:

```text
authority
scope
ambiguity
irreversibility
```

Practical safety rules:

```text
use a dedicated working folder
back up irreplaceable files
test workflows on copies first
be precise with destructive verbs
name prompt bounds
use scheduled tasks for drafts initially
read Claude's plan
watch for unexpected patterns
approve confirmation prompts deliberately
```

---

## 21. Evals Validate Skills Before Sharing

A skill or plugin is a reusable workflow artifact.

Before relying on it or sharing it, it should be tested.

In this course, an eval is a lightweight try-out.

```text
realistic prompt in
→ skill output out
→ compare against baseline
→ give feedback
→ revise skill
```

The eval compares:

```text
Claude with the skill
Claude without the skill
```

The key questions:

```text
Is the skill version the one I would actually send?

If not, what is missing, wrong, too long, too vague, or off-format?
```

Good feedback is specific.

```text
It skipped the executive summary.
The tone is too formal.
Action items need owners and dates.
Lead with decisions, not background.
Keep the recap under 150 words.
```

The eval loop:

```text
Run eval
→ compare with-skill vs without-skill
→ give feedback
→ update skill
→ re-run eval
→ ship when useful
```

The bar is not perfection.

The bar is meaningful improvement over baseline on the cases that matter.

---

## 22. Shared Plugins Need Governance

Once a workflow works for more than one person, it can become team infrastructure.

The usual sharing unit is a plugin.

```text
personal workflow
→ validated skill
→ bundled plugin
→ organization marketplace
→ team installation
```

Enterprise distribution modes:

```text
Available:
  appears in directory, users install optionally

Installed by default:
  installed automatically, users can turn it off

Required:
  installed and stays on
```

Shared plugins need maintenance.

Good habits:

```text
define an owner
keep examples current
version changes deliberately
collect feedback
run evals before updates
document limitations
review connector permissions
avoid broad access by default
```

A plugin is not just an artifact.

It is an operational workflow asset.

---

# Course-Specific Sections

## Cowork as Supervised Delegation

Cowork is best understood as supervised delegation.

```text
User defines the desired outcome.
Claude plans and executes.
User approves, steers, and reviews.
```

The user does not manually stitch every step together, but also does not disappear from the loop.

This middle ground is the core Cowork model.

```text
Cowork
=
delegated execution + human control loop
```

---

## Cowork Task Anatomy

A strong Cowork task has this shape:

```text
Goal:
  What should be achieved?

Deliverable:
  What artifact should be produced?

Workspace:
  Which folder is in scope?

Inputs:
  Which files, apps, folders, date ranges, channels, or documents matter?

Nuance:
  Audience, decision context, assumptions, exclusions, style, and verification rules.

Permissions:
  What can Claude do automatically?
  What requires approval?

Review:
  What makes the final output acceptable?
```

This turns vague intent into executable work.

---

## Cowork Context Layers

Cowork uses several layers of context.

```text
Global instructions
→ stable user/work preferences

Projects
→ workstream context and memory

Skills
→ reusable process playbooks

Plugins
→ bundled role/team expertise

Connectors
→ external application context

Working folder
→ local file workspace

Chrome
→ browser-accessible tools and dashboards

M365
→ native document work surfaces
```

Good Cowork use depends on placing knowledge at the correct layer.

```text
If it applies everywhere:
  global instructions

If it applies to one workstream:
  project

If it is a repeatable process:
  skill

If it is a role/team toolkit:
  plugin

If it lives behind a browser:
  Chrome

If it lives inside Office files:
  M365
```

---

## Cowork Safety Model

The safety model is not only about confirmations.

It includes:

```text
workspace boundary
connector scope
permission mode
prompt specificity
approval gates
backup strategy
draft-first automation
human review
```

The strongest practical rule:

```text
Design the task so one bad click is survivable.
```

---

## Cowork Versus Chat Versus Code

The course repeatedly emphasizes choosing the right surface.

```text
Chat:
  turn-by-turn thinking, drafting, brainstorming, and bounded questions

Cowork:
  multi-step delegated work across files, tools, apps, and deliverables

Code:
  agentic software development inside a repository
```

The correct choice depends on the shape of the work.

```text
Question or idea:
  Chat

Deliverable across files/apps:
  Cowork

Software repository task:
  Code
```

---

# Force Agentic Command Lab Translation

The course can be summarized as:

```text
Claude Cowork
=
Delegated Knowledge-Work Agent With Workspace, Tools, Memory, Skills, and Safety Controls
```

For the command lab, Cowork provides a practical blueprint for turning agentic prompting into repeatable task systems.

---

## 1. Delegation Requires a Complete Task Contract

A Cowork prompt should not just ask for an answer.

It should define a task contract.

```text
Goal
Deliverable
Inputs
Workspace
Nuance
Permissions
Review criteria
```

Command-lab rule:

```text
No autonomous task without an operating envelope.
```

---

## 2. Context Should Live at the Right Layer

Repeated context should not be retyped forever.

Use this placement rule:

```text
Stable personal preference:
  global instructions

Recurring workstream:
  project

Repeated workflow:
  skill

Team-level workflow package:
  plugin

Browser-only data:
  Chrome

Office-file work:
  M365
```

The command-lab version:

```text
Stop re-explaining stable context.
Store it at the lowest reusable layer where it remains true.
```

---

## 3. Safe Autonomy Requires Bounds

Cowork shows that useful autonomy is not unbounded autonomy.

A safe command should specify:

```text
allowed files
forbidden files
allowed tools
forbidden actions
draft versus send/share mode
approval gates
backup requirements
validation criteria
```

Command-lab safe delegation template:

```text
Goal:
  What should be done?

Workspace:
  Which folder/files/apps are in scope?

Read permissions:
  What may Claude inspect?

Write permissions:
  What may Claude edit/create?

Forbidden actions:
  What must not happen?

Approval gates:
  What requires explicit confirmation?

Draft mode:
  Should outputs be draft-only first?

Validation:
  How will we judge correctness?

Rollback:
  Are originals backed up or copied?
```

---

## 4. Skills Need Evals Before They Become Infrastructure

A skill is a reusable process.

A reusable process should be tested.

Command-lab eval template:

```text
Skill:
  What process is being tested?

Test prompts:
  Realistic requests users might provide.

Baseline:
  Claude without the skill.

Candidate:
  Claude with the skill.

Rubric:
  What good output requires.

Feedback:
  One specific improvement at a time.

Decision:
  Ship, revise, or document limitation.
```

This turns prompt craft into engineering validation.

---

## 5. Plugins Are Team Infrastructure

Plugins represent the point where personal workflow becomes shared infrastructure.

```text
one-off prompt
→ repeated personal process
→ skill
→ validated skill
→ plugin
→ team distribution
```

Command-lab rule:

```text
Do not share a workflow until it has examples, evals, owner, permissions reviewed, known limitations, and an update path.
```

---

# Relationship to the Markovian Unit-Step Protocol

Claude Cowork maps very cleanly onto the Markovian Unit-Step Protocol.

MUSP is:

```text
Observe
→ Act
→ Verify
→ Update State
→ Repeat
```

Cowork applies that pattern to knowledge-work delegation.

---

## Cowork Task Execution in MUSP

```text
Observe:
  Claude reads prompt, folder, project context, connectors, and task state.

Act:
  Claude plans, searches, edits, drafts, formats, or uses tools.

Verify:
  Claude asks clarifying questions, shows progress, requests approval, and previews output.

Update State:
  Files are edited or saved, project memory updates, user steering modifies direction.

Repeat:
  Continue until the deliverable is complete and reviewed.
```

Formal view:

```text
s_t = workspace + connector context + project memory + task progress

a_t = next Cowork action

v_t = approval / user steering / artifact review

s_{t+1} = updated workspace and task state
```

Cowork is supervised autonomy.

```text
Cowork
=
MUSP loop with human approval and steering signals
```

---

## Global Instructions in MUSP

```text
Observe:
  Task begins.

Act:
  Claude applies stable user preferences.

Verify:
  Output matches default style and format.

Update State:
  Repeated corrections become global-instruction candidates.
```

---

## Projects in MUSP

```text
Observe:
  Claude reads project instructions, context, and memory.

Act:
  Executes task inside the project workspace.

Verify:
  Checks against project instructions and prior decisions.

Update State:
  Project memory accumulates new decisions and context.
```

Project memory keeps the loop from starting cold every time.

---

## Skills in MUSP

```text
Observe:
  Task matches a skill trigger.

Act:
  Claude loads the skill and follows its procedure.

Verify:
  Output is checked against references, examples, or rubrics.

Update State:
  Skill is revised if output misses the standard.
```

A skill is a reusable local policy for a known task class.

---

## Plugins in MUSP

```text
Observe:
  Role or workflow package is installed.

Act:
  Claude selects the relevant bundled skill, connector, or subagent.

Verify:
  Output follows team workflow and review criteria.

Update State:
  Plugin is customized or maintained over time.
```

Plugins make MUSP portable across a team.

---

## Safety in MUSP

```text
Observe:
  Identify workspace, permissions, prompt bounds, and risk.

Act:
  Claude executes bounded task steps.

Verify:
  Read plan, monitor progress, inspect confirmations.

Update State:
  Approve, steer, stop, or revise.

Repeat:
  Continue only if actions remain within bounds.
```

Safety is continuous verification, not a one-time setting.

---

## Skill Evals in MUSP

```text
Observe:
  Compare with-skill and without-skill outputs.

Act:
  Give targeted feedback.

Verify:
  Re-run eval and inspect improvement.

Update State:
  Revise skill instructions, assets, references, or scripts.

Repeat:
  Continue until the skill clears the useful-quality bar.
```

This is the same loop applied to workflow validation.

---

# Personal Additions

## 1. Cowork as a Bounded Office Robot

Cowork feels like the knowledge-work version of a robot operating in a bounded workspace.

```text
Folder:
  workspace boundary

Connectors:
  external sensors/tools

Permissions:
  safety interlocks

Progress panel:
  telemetry

Clarifying questions:
  state-estimation corrections

Mid-task steering:
  feedback control

Final artifact:
  mission output
```

The main beginner mistake is using Cowork like Chat.

Better mental shift:

```text
Do not ask Cowork for an answer.
Assign Cowork a job.
```

---

## 2. Cowork as Task-Level Agent Architecture

From an agentic systems perspective, Cowork is a task-level execution architecture.

```text
Input:
  user goal + folder + connectors + permissions

Planner:
  Claude creates task plan

Executor:
  Claude reads/writes files and uses tools

Supervisor:
  user answers questions, approves actions, and steers

Output:
  saved artifact
```

Control analogy:

```text
Reference command:
  desired deliverable

State:
  current files, connector data, task progress

Control action:
  Claude's plan step / file edit / app action

Observation:
  progress panel, clarifying questions, output preview

Feedback:
  user approval, steering, review comments

Terminal condition:
  deliverable saved and accepted
```

Mathematical view:

```text
s_t = active workspace state

g = requested deliverable

a_t = Claude-selected action

y_t = observed progress/output

u_t = user steering signal

s_{t+1} = F(s_t, a_t, u_t)
```

---

## 3. Context Placement Is Systems Design

This course is also about where knowledge should live.

Bad workflow:

```text
Repeat the same context every time.
Correct the same mistakes every time.
Paste the same templates every time.
Ask Claude to rediscover the same project history every time.
```

Good workflow:

```text
Global stable preferences
→ global instructions

Recurring workstream
→ project

Repeated process
→ skill

Team toolkit
→ plugin

Browser-only data
→ Chrome

Office-file refinement
→ M365
```

Central operating principle:

```text
Context should live at the lowest reusable layer where it remains true.
```

Tiny math version:

```text
For context item c:
  place(c) = argmin_layer(repetition_cost + retrieval_cost + staleness_risk)
```

If context applies everywhere, store it high.

If it applies narrowly, store it low.

If it changes often, keep it close to the active task.

---

## 4. Autonomy Increases the Value of Constraints

The final safety lesson is the strongest engineering lesson.

```text
Autonomy increases the value of constraints.
```

The more Claude can do, the more precisely the user must define:

```text
scope
permissions
success
failure
review
rollback
```

Robotics analogy:

```text
A robot with more actuators needs stronger safety envelopes.

An agent with more tools needs stronger operating boundaries.
```

Cowork is not about blind trust.

It is about controlled delegation.

---

# Overall Assessment

## Strengths

- Clear explanation of Cowork as delegation rather than chat.
- Strong distinction between Chat, Cowork, and Code.
- Practical explanation of folders as working boundaries.
- Useful guidance on connectors and how they expand context.
- Good treatment of permission modes and user control.
- Strong examples of Cowork-shaped tasks.
- Practical prompt structure: deliverable, inputs, and nuance.
- Useful explanation of clarifying questions as context-gap closure.
- Strong emphasis on mid-task steering rather than waiting for final output.
- Good explanation of global instructions and projects as persistent context layers.
- Clear treatment of skills as reusable playbooks.
- Strong explanation of plugins as role/team workflow bundles.
- Useful inclusion of subagents as focused workers for independent task pieces.
- Practical introduction to Chrome as the bridge for browser-only work.
- Clear distinction between Cowork and Claude for Microsoft 365.
- Strong safety module with realistic workspace, prompt, and approval practices.
- Useful eval framing for validating skills before sharing.
- Good team-scaling model through organization plugin marketplaces.

## Weaknesses

- The course remains high-level around the exact technical implementation of Cowork.
- It does not deeply explain how local file access, sync, or sandboxing works under the hood.
- Chrome access is powerful but the security discussion could be more formal.
- The course mentions that Cowork activity is not captured in some audit systems, but does not deeply map compliance alternatives.
- Skill evals are introduced practically, but not connected to formal evaluation metrics or regression-test workflows.
- Plugin lifecycle management is introduced, but deeper versioning and rollback strategy would be useful.
- Scheduled task behavior is clear, but failure modes, retries, and logging are not deeply explored.
- Subagent behavior is introduced conceptually, but not deeply diagrammed.
- Enterprise governance receives only introductory treatment.

---

# Quiz Results

The final assessment contained eight questions.

Results:

```text
Correct Answers: 8 / 8
Score: 100%
Elapsed Time: 2 minutes
```

Topics tested:

- subagents working on separate independent pieces with focused context.
- Cowork project memory versus broader Chat memory.
- relationship between skills and plugins.
- scheduled tasks when the computer is asleep.
- how context carries between Cowork tasks.
- reviewing Cowork output before acting on it.
- working folder read/write capability.
- choosing Cowork rather than Chat for file-based deliverable work.

---

# Final Takeaway

Introduction to Claude Cowork shows that Claude can move beyond conversation into supervised delegated work.

The central model is:

```text
Cowork
=
Claude working inside a bounded workspace across files, apps, tools, browser surfaces, and deliverables.
```

The course builds the full operating stack:

```text
Working folder
→ local file workspace

Connectors
→ app context and actions

Permissions
→ safety gates

Global instructions
→ stable user preferences

Projects
→ workstream memory and context

Skills
→ reusable process playbooks

Plugins
→ bundled team expertise

Chrome
→ browser-based work surface

M365
→ in-document assistance

Evals
→ validation before reuse

Marketplace sharing
→ team distribution
```

The strongest practical rule is:

```text
Use Chat for thinking.
Use Cowork for delegated deliverables.
Use Code for software work inside repositories.
```

The strongest safety rule is:

```text
Delegate intelligently, constrain carefully, evaluate honestly, and review before shipping.
```

In one sentence:

```text
Claude Cowork turns Claude from a conversational assistant into a supervised work-execution agent for real files, real tools, and real deliverables.
```

---