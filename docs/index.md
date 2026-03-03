# GitHub Copilot Custom Agents — Template & Guide

> A practical reference project for building, composing, and deploying **custom agents** for GitHub Copilot Chat using the `.agent.md` file format.

---

## Table of Contents

1. [What Are Custom Agents?](#what-are-custom-agents)
2. [Project Structure](#project-structure)
3. [Quick Start](#quick-start)
4. [The `.agent.md` File Format](#the-agentmd-file-format)
5. [Example Agents](#example-agents)
6. [Composing Agents Together](#composing-agents-together)
7. [Glossary of AI Terms](#glossary-of-ai-terms)
8. [Resources & Further Reading](#resources-further-reading)

---

## What Are Custom Agents?

Custom agents let you configure GitHub Copilot Chat to adopt **different personas** tailored to specific development roles and tasks. Instead of building an HTTP server, you simply write a **Markdown file** with a `.agent.md` extension that defines:

- **Instructions** — what the agent does and how it behaves
- **Tools** — which tools the agent can use (file editing, terminal, MCP servers, etc.)
- **Model** — which LLM powers the agent
- **Handoffs** — how agents transition to other agents in multi-step workflows

Agents are invoked by selecting them from the **agents dropdown** in VS Code Copilot Chat, or they can be called automatically as **subagents** by other agents.

### How It Works

```
┌─────────────────────────────────────────────────────────────┐
│  VS Code Copilot Chat                                       │
│                                                             │
│  Agent picker: [ code-reviewer ▼ ]                          │
│                                                             │
│  User: "Review my auth module for security issues"          │
│         │                                                   │
│         ▼                                                   │
│  ┌──────────────────────────────────┐                       │
│  │  code-reviewer.agent.md          │                       │
│  │  ─────────────────────────       │                       │
│  │  description: Code reviewer      │                       │
│  │  tools: [search, fetch, ...]     │                       │
│  │                                  │                       │
│  │  Instructions:                   │                       │
│  │  "You are a senior code          │                       │
│  │   reviewer. Check for..."        │                       │
│  └──────────────┬───────────────────┘                       │
│                 │                                           │
│                 ▼                                           │
│  LLM generates response using the                          │
│  agent's instructions + tools                              │
└─────────────────────────────────────────────────────────────┘
```

> **Note:** Custom agents require **VS Code 1.106+** (released 2025). They were previously called "custom chat modes" (`.chatmode.md`).

---

## Project Structure

```
CoPilot_Agent_Template/
├── README.md                              # This file
├── .github/
│   ├── copilot-instructions.md            # Global Copilot instructions for this project
│   └── agents/                            # Custom agent definitions
│       ├── code-reviewer.agent.md         # Security & quality reviewer
│       ├── planner.agent.md               # Implementation planner
│       ├── implementer.agent.md           # Code implementer (handoff from planner)
│       ├── test-writer.agent.md           # Test generation specialist
│       ├── doc-writer.agent.md            # Documentation writer
│       └── orchestrator.agent.md          # Multi-agent orchestrator
└── docs/
    ├── glossary.md                        # AI & tooling glossary
    ├── composing-agents.md                # How agents work together
    └── resources.md                       # Links & further reading
```

---

## Quick Start

### Prerequisites

- **VS Code 1.106+** (or VS Code Insiders)
- A **GitHub Copilot** subscription (Free, Pro, or Enterprise)
- The **GitHub Copilot Chat** extension installed

### 1. Clone This Repo

```bash
git clone <your-repo-url>
cd CoPilot_Agent_Template
```

### 2. Open in VS Code

```bash
code .
```

### 3. Select an Agent

1. Open the Copilot Chat panel (`Ctrl+Alt+I` / `Cmd+Alt+I`)
2. Click the **agents dropdown** at the top of the chat
3. Select one of the custom agents (e.g., `code-reviewer`)
4. Start chatting — the agent's instructions and tools are now active

> **Tip:** Type `/agents` in chat to quickly open the Configure Custom Agents menu.

### 4. Create Your Own

Run the command **Chat: New Custom Agent** (`Ctrl+Shift+P`) or manually create a `.agent.md` file in `.github/agents/`.

---

## The `.agent.md` File Format

Every custom agent is a Markdown file with an optional **YAML frontmatter** header and a **Markdown body** containing instructions.

### Minimal Example

```markdown
---
description: A friendly greeting agent
tools: []
---

You are a friendly assistant. Greet the user warmly and help them
with basic questions about this project.
```

### Full Example with All Options

```markdown
---
# Display name (optional — defaults to filename)
name: Code Reviewer

# Shown as placeholder text in the chat input
description: Reviews code for security, performance, and best practices

# Hint text shown in chat input
argument-hint: Paste code or describe what to review

# Tools this agent can use (see tool list below)
tools:
  - search           # Workspace search
  - read             # Read files
  - listDirectory    # List folder contents
  - fetch            # Fetch web pages
  - githubRepo       # GitHub repo context

# Available subagents
agents:
  - test-writer      # Can delegate to test-writer agent

# Preferred language model (single or prioritised list)
model:
  - claude-sonnet-4  # Try Claude first
  - gpt-4.1          # Fall back to GPT-4.1

# Handoffs — buttons shown after response completes
handoffs:
  - label: Write Tests
    agent: test-writer
    prompt: Write tests for the issues identified above.
    send: false
  - label: Fix Issues
    agent: implementer
    prompt: Fix the issues identified in the review above.
    send: false
---

You are a senior code reviewer. When reviewing code:

1. Check for **security vulnerabilities** (injection, auth issues, data exposure)
2. Identify **performance problems** (N+1 queries, unnecessary allocations)
3. Review **error handling** (missing try/catch, unhandled promises)
4. Assess **code style** and adherence to project conventions
5. Suggest **improvements** with concrete code examples

Use #tool:search to find related code in the workspace.
Use #tool:read to examine specific files.

Always provide:
- A severity rating (🔴 Critical, 🟡 Warning, 🟢 Info)
- The file and line number
- A suggested fix
```

### YAML Frontmatter Reference

| Field | Type | Description |
|-------|------|-------------|
| `name` | `string` | Display name (defaults to filename) |
| `description` | `string` | Shown as placeholder text in chat input |
| `argument-hint` | `string` | Hint text for user guidance |
| `tools` | `string[]` | List of available tools (see below) |
| `agents` | `string[]` | Subagents this agent can delegate to. Use `*` for all, `[]` for none |
| `model` | `string \| string[]` | Preferred LLM(s), tried in order |
| `user-invokable` | `boolean` | Show in agents dropdown (default: `true`) |
| `disable-model-invocation` | `boolean` | Prevent use as subagent (default: `false`) |
| `handoffs` | `object[]` | Buttons for transitioning to other agents |
| `handoffs[].label` | `string` | Button text |
| `handoffs[].agent` | `string` | Target agent identifier |
| `handoffs[].prompt` | `string` | Prompt sent to target agent |
| `handoffs[].send` | `boolean` | Auto-submit the prompt (default: `false`) |
| `handoffs[].model` | `string` | Model override for the handoff |

### Common Tools

| Tool | Description |
|------|-------------|
| `editFiles` | Create and edit files in the workspace |
| `search` | Workspace text search |
| `read` | Read file contents |
| `listDirectory` | List folder contents |
| `runInTerminal` | Run terminal commands |
| `fetch` | Fetch web content |
| `githubRepo` | Access GitHub repository info |
| `useMcp` | Use MCP server tools |
| `agent` | Delegate to subagents |
| `<mcp-server>/*` | All tools from a specific MCP server |

---

## Example Agents

This project includes six example agents in `.github/agents/`:

| Agent | File | Description | Use Case |
|-------|------|-------------|----------|
| Code Reviewer | `code-reviewer.agent.md` | Reviews code for security & quality | Code review before merge |
| Planner | `planner.agent.md` | Creates implementation plans | Starting a new feature |
| Implementer | `implementer.agent.md` | Writes code from plans | Implementing features |
| Test Writer | `test-writer.agent.md` | Generates test cases | Adding test coverage |
| Doc Writer | `doc-writer.agent.md` | Writes documentation | Documenting code |
| Orchestrator | `orchestrator.agent.md` | Coordinates other agents | Complex multi-step tasks |

### How to Use Them

1. Open Copilot Chat in VS Code
2. Select an agent from the dropdown
3. Describe your task
4. The agent responds using its specialised instructions and tools
5. Use **handoff buttons** to transition to the next agent in the workflow

---

## Composing Agents Together

Agents become powerful when combined. See [Composing Agents](composing-agents.md) for full details on:

- **Handoffs** — buttons that transition between agents with context
- **Subagents** — agents that delegate to other agents autonomously
- **Workflow pipelines** — Plan → Implement → Test → Review chains
- **MCP integration** — agents sharing external tool servers

### Quick Example: Plan → Implement Workflow

```
┌─────────────────┐    Handoff     ┌──────────────────┐
│ planner.agent.md │ ─────────────▶│ implementer      │
│                  │  "Implement   │  .agent.md       │
│ Creates a plan   │   the plan"   │                  │
│ with steps       │               │ Writes the code  │
└─────────────────┘               └──────────────────┘
```

The planner agent creates a step-by-step plan. When done, a **"Start Implementation"** button appears. Clicking it switches to the implementer agent with the plan as context.

---

## Glossary of AI Terms

A comprehensive glossary covering MCP, Playwright, RAG, LLMs, and 30+ more terms is in the [Glossary](glossary.md).

---

## Resources & Further Reading

Curated links to official docs, tutorials, and community projects are in [Resources](resources.md).

---

## License

This project is provided as a learning resource. Use it however you like.
