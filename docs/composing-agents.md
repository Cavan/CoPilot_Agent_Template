# Composing Agents Together

One of the most powerful features of `.agent.md` custom agents is the ability to **compose** them — having agents work together in multi-step workflows. This document covers all the composition patterns available.

---

## Table of Contents

1. [Handoffs](#handoffs)
2. [Subagents](#subagents)
3. [Workflow Patterns](#workflow-patterns)
4. [MCP Integration](#mcp-integration)
5. [Best Practices](#best-practices)

---

## Handoffs

**Handoffs** are interactive buttons that appear after an agent completes its response. They let the user transition to another agent with context and a pre-filled prompt.

### How Handoffs Work

```
┌──────────────────────────┐
│  planner.agent.md        │
│                          │
│  User: "Plan a login     │
│         feature"         │
│                          │
│  Agent creates plan...   │
│                          │
│  ┌─────────────────────┐ │
│  │ Start Implementation│ │ ◄── Handoff button
│  └─────────────────────┘ │
│  ┌─────────────────────┐ │
│  │ Write Tests First   │ │ ◄── Another handoff
│  └─────────────────────┘ │
└──────────────────────────┘
         │
         │ User clicks "Start Implementation"
         ▼
┌──────────────────────────┐
│  implementer.agent.md    │
│                          │
│  Receives the plan as    │
│  context + pre-filled    │
│  prompt: "Implement the  │
│  plan outlined above."   │
│                          │
│  Starts writing code...  │
└──────────────────────────┘
```

### Defining Handoffs

Add a `handoffs` array to your agent's YAML frontmatter:

```yaml
---
description: Creates implementation plans
tools:
  - search
  - read
handoffs:
  - label: Start Implementation       # Button text
    agent: implementer                 # Target agent (filename without .agent.md)
    prompt: Implement the plan above.  # Prompt sent to target
    send: false                        # false = user can review/edit before sending

  - label: Write Tests First
    agent: test-writer
    prompt: Write failing tests based on the plan above.
    send: false
---
```

### Handoff Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | `string` | required | The text shown on the button |
| `agent` | `string` | required | The target agent's identifier (filename without `.agent.md`) |
| `prompt` | `string` | required | The prompt text sent to the target agent |
| `send` | `boolean` | `false` | If `true`, the prompt auto-submits. If `false`, the user can review it first |
| `model` | `string` | — | Override the language model for the handoff |

### Tips for Effective Handoffs

- **Be descriptive in the prompt** — Reference "above" so the target agent knows to use the prior conversation as context
- **Use `send: false` for important transitions** — Let the user review and edit the prompt before sending
- **Use `send: true` for automated pipelines** — When you want a seamless, automatic flow
- **Limit handoff buttons to 2–3** — Too many options cause decision fatigue

---

## Subagents

**Subagents** allow one agent to autonomously delegate tasks to another agent without user interaction. The parent agent decides when to call the child agent and integrates the result.

### How Subagents Work

```
┌────────────────────────────────────────────┐
│  orchestrator.agent.md                     │
│                                            │
│  User: "Build a login page with tests"     │
│                                            │
│  Agent decides to:                         │
│    1. Delegate to planner (subagent)       │
│    2. Delegate to implementer (subagent)   │
│    3. Delegate to test-writer (subagent)   │
│                                            │
│  Results flow back automatically           │
└────────────────────────────────────────────┘
```

### Defining Subagents

Use the `agents` property in your YAML frontmatter, and include `agent` in your tools list:

```yaml
---
description: Coordinates multi-step workflows
tools:
  - search
  - read
  - agent           # Required — enables subagent delegation
agents:
  - planner          # Allow delegation to planner
  - implementer      # Allow delegation to implementer
  - test-writer      # Allow delegation to test-writer
---
```

### Subagent Options

| Value | Description |
|-------|-------------|
| `['agent-name']` | Allow specific agents only |
| `['*']` | Allow delegation to any available agent |
| `[]` | Prevent any subagent use |

### Handoffs vs Subagents

| Feature | Handoffs | Subagents |
|---------|----------|-----------|
| User interaction | User clicks a button | Automatic, no user input |
| Control | User reviews prompt before sending | Agent decides autonomously |
| Visibility | Clearly visible transition | Happens within the agent's response |
| Best for | Sequential workflows with review steps | Autonomous multi-step tasks |

---

## Workflow Patterns

### Pattern 1: Sequential Pipeline (Plan → Implement → Test → Review)

The most common pattern. Each agent completes its work, then offers a handoff to the next.

```
planner ──handoff──▶ implementer ──handoff──▶ test-writer ──handoff──▶ code-reviewer
```

**How to set up:**

Each agent defines a handoff to the next:

```yaml
# planner.agent.md
handoffs:
  - label: Start Implementation
    agent: implementer
    prompt: Implement the plan above.

# implementer.agent.md
handoffs:
  - label: Write Tests
    agent: test-writer
    prompt: Write tests for the code above.

# test-writer.agent.md
handoffs:
  - label: Review Code
    agent: code-reviewer
    prompt: Review the implementation and tests above.
```

### Pattern 2: Hub and Spoke (Orchestrator)

A central orchestrator agent delegates to specialists as needed.

```
                  ┌──────────┐
                  │planner   │
                  └────▲─────┘
                       │
┌──────────┐    ┌──────┴──────┐    ┌──────────┐
│test-writer│◄──│ orchestrator │──►│implementer│
└──────────┘    └──────┬──────┘    └──────────┘
                       │
                  ┌────▼─────┐
                  │code-     │
                  │reviewer  │
                  └──────────┘
```

**How to set up:**

The orchestrator lists all agents as subagents:

```yaml
# orchestrator.agent.md
tools:
  - agent
agents:
  - planner
  - implementer
  - test-writer
  - code-reviewer
  - doc-writer
```

### Pattern 3: TDD Loop (Test → Implement → Verify)

Write tests first, then implement to make them pass.

```
test-writer ──handoff──▶ implementer ──handoff──▶ test-writer (verify)
     ▲                                                  │
     └───────────────── if tests fail ──────────────────┘
```

### Pattern 4: Read-Only Analysis Chain

Some agents should never modify files. Chain read-only agents for analysis.

```yaml
# All agents in this chain use only read-only tools:
tools:
  - search
  - read
  - listDirectory
  - fetch
# No editFiles or runInTerminal
```

---

## MCP Integration

Agents can use tools provided by **MCP (Model Context Protocol) servers**. This lets agents interact with external services like databases, APIs, and DevOps tools.

### Using MCP Tools in Agents

Reference MCP server tools in your agent's `tools` list:

```yaml
---
description: Database administrator agent
tools:
  - search
  - read
  - postgres-mcp/*     # All tools from the postgres MCP server
  - jira-mcp/search    # Specific tool from the jira MCP server
---
```

### Setting Up MCP Servers

MCP servers are configured in VS Code settings (`.vscode/settings.json`) or in `mcp.json`:

```json
// .vscode/mcp.json
{
  "servers": {
    "postgres-mcp": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres", "postgresql://localhost:5432/mydb"]
    },
    "github-mcp": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
      }
    }
  }
}
```

### Example: Agent with MCP Tools

```yaml
---
name: DBA Agent
description: Database administration and query helper
tools:
  - read
  - search
  - postgres-mcp/*
---

You are a database administrator. Use the PostgreSQL MCP tools to:
- Run queries
- Inspect table schemas
- Suggest index improvements
- Analyse query performance

Always explain queries before running them. Never run DELETE or DROP
operations without explicit user confirmation.
```

---

## Best Practices

### 1. Keep Agents Focused
Each agent should have one clear role. A "do everything" agent is no better than the default.

### 2. Use Read-Only Tools for Analysis Agents
Agents that analyse or review code should NOT have `editFiles` or `runInTerminal` tools — this prevents accidental changes.

### 3. Write Clear Handoff Prompts
The handoff prompt is what connects two agents. Make it reference the conversation context:
```yaml
# Good
prompt: Implement the plan outlined above, following the architecture decisions made.

# Bad
prompt: Do the thing.
```

### 4. Limit Subagent Scope
Don't use `agents: ['*']` unless necessary. Be explicit about which agents can be delegated to.

### 5. Test Your Workflows
Try the full handoff chain end-to-end. Common issues:
- Missing context between agents
- Wrong tools for the downstream agent
- Circular handoffs (A → B → A)

### 6. Version Control Your Agents
Since `.agent.md` files are just Markdown, they work perfectly with Git. Your team's agent configurations evolve with the project.

---

*See [resources.md](resources.md) for links to official documentation and community examples.*
