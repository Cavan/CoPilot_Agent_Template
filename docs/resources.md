# Resources & Further Reading

A curated list of official documentation, tutorials, and community resources for building GitHub Copilot custom agents and related AI developer tooling.

---

## Table of Contents

1. [Official Documentation](#official-documentation)
2. [Custom Agents](#custom-agents)
3. [MCP (Model Context Protocol)](#mcp-model-context-protocol)
4. [Prompt Engineering](#prompt-engineering)
5. [Agent Skills](#agent-skills)
6. [VS Code Extensions & Tools](#vs-code-extensions-tools)
7. [Community & Learning](#community-learning)

---

## Official Documentation

### GitHub Copilot
| Resource | Link |
|----------|------|
| Copilot Overview | [docs.github.com/copilot](https://docs.github.com/en/copilot) |
| Copilot Chat | [VS Code: Chat Overview](https://code.visualstudio.com/docs/copilot/chat/copilot-chat) |
| Agents Overview | [VS Code: Agents](https://code.visualstudio.com/docs/copilot/agents/overview) |
| Customise AI in VS Code | [VS Code: Customization](https://code.visualstudio.com/docs/copilot/customization/overview) |
| Copilot Pricing & Plans | [github.com/features/copilot](https://github.com/features/copilot) |

### VS Code
| Resource | Link |
|----------|------|
| VS Code Docs | [code.visualstudio.com/docs](https://code.visualstudio.com/docs) |
| VS Code Release Notes | [code.visualstudio.com/updates](https://code.visualstudio.com/updates) |
| VS Code API Reference | [code.visualstudio.com/api](https://code.visualstudio.com/api) |

---

## Custom Agents

### Official Guides
| Resource | Link |
|----------|------|
| **Custom Agents in VS Code** | [code.visualstudio.com/docs/copilot/customization/custom-agents](https://code.visualstudio.com/docs/copilot/customization/custom-agents) |
| Custom Instructions | [code.visualstudio.com/docs/copilot/customization/custom-instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions) |
| Prompt Files (Slash Commands) | [code.visualstudio.com/docs/copilot/customization/prompt-files](https://code.visualstudio.com/docs/copilot/customization/prompt-files) |
| Tools in Chat | [code.visualstudio.com/docs/copilot/agents/agent-tools](https://code.visualstudio.com/docs/copilot/agents/agent-tools) |
| Subagents | [code.visualstudio.com/docs/copilot/agents/subagents](https://code.visualstudio.com/docs/copilot/agents/subagents) |
| Background Agents | [code.visualstudio.com/docs/copilot/agents/background-agents](https://code.visualstudio.com/docs/copilot/agents/background-agents) |
| Cloud Agents | [code.visualstudio.com/docs/copilot/agents/cloud-agents](https://code.visualstudio.com/docs/copilot/agents/cloud-agents) |
| Organisation Custom Agents | [docs.github.com: Create custom agents](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents) |

### Key Concepts
- **`.agent.md` file** — A Markdown file with YAML frontmatter that defines an agent's persona, tools, and instructions
- **Handoffs** — Interactive buttons that transition between agents with pre-filled prompts
- **Subagents** — Agents that delegate tasks to other agents autonomously
- **Tool restrictions** — Limiting which tools an agent can use (e.g., read-only for reviewers)

---

## MCP (Model Context Protocol)

### Official Resources
| Resource | Link |
|----------|------|
| MCP Specification | [spec.modelcontextprotocol.io](https://spec.modelcontextprotocol.io/) |
| MCP Documentation | [modelcontextprotocol.io](https://modelcontextprotocol.io/) |
| MCP GitHub Org | [github.com/modelcontextprotocol](https://github.com/modelcontextprotocol) |
| VS Code: MCP Servers | [code.visualstudio.com/docs/copilot/customization/mcp-servers](https://code.visualstudio.com/docs/copilot/customization/mcp-servers) |

### Popular MCP Servers
| Server | Description | Link |
|--------|-------------|------|
| Filesystem | Read/write local files | [npm: @modelcontextprotocol/server-filesystem](https://www.npmjs.com/package/@modelcontextprotocol/server-filesystem) |
| GitHub | GitHub API access | [npm: @modelcontextprotocol/server-github](https://www.npmjs.com/package/@modelcontextprotocol/server-github) |
| PostgreSQL | Database querying | [npm: @modelcontextprotocol/server-postgres](https://www.npmjs.com/package/@modelcontextprotocol/server-postgres) |
| Playwright | Browser automation | [github.com/anthropics/mcp-playwright](https://github.com/anthropics/mcp-playwright) |
| Fetch | HTTP requests | [npm: @modelcontextprotocol/server-fetch](https://www.npmjs.com/package/@modelcontextprotocol/server-fetch) |
| Memory | Persistent context | [npm: @modelcontextprotocol/server-memory](https://www.npmjs.com/package/@modelcontextprotocol/server-memory) |

---

## Prompt Engineering

| Resource | Link |
|----------|------|
| VS Code: Prompt Engineering Guide | [code.visualstudio.com/docs/copilot/guides/prompt-engineering-guide](https://code.visualstudio.com/docs/copilot/guides/prompt-engineering-guide) |
| VS Code: Prompt Examples | [code.visualstudio.com/docs/copilot/chat/prompt-examples](https://code.visualstudio.com/docs/copilot/chat/prompt-examples) |
| OpenAI: Prompt Engineering Best Practices | [platform.openai.com/docs/guides/prompt-engineering](https://platform.openai.com/docs/guides/prompt-engineering) |
| Anthropic: Prompt Engineering | [docs.anthropic.com/claude/docs/prompt-engineering](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) |

---

## Agent Skills

Agent Skills is an open standard for reusable AI capabilities that work across multiple tools.

| Resource | Link |
|----------|------|
| Agent Skills Specification | [agentskills.io](https://agentskills.io/) |
| VS Code: Agent Skills | [code.visualstudio.com/docs/copilot/customization/agent-skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills) |

---

## VS Code Extensions & Tools

### Essential Extensions
| Extension | Description |
|-----------|-------------|
| [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) | AI code completion and chat |
| [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) | Chat interface for Copilot |

### Development Tools
| Tool | Description | Link |
|------|-------------|------|
| ngrok | Expose local services publicly (useful for MCP servers) | [ngrok.com](https://ngrok.com/) |
| Playwright | Browser automation & testing | [playwright.dev](https://playwright.dev/) |

---

## Community & Learning

### Blogs & Articles
| Resource | Link |
|----------|------|
| GitHub Blog: Copilot | [github.blog/tag/github-copilot](https://github.blog/tag/github-copilot/) |
| VS Code Blog | [code.visualstudio.com/blogs](https://code.visualstudio.com/blogs) |

### Video Resources
| Resource | Link |
|----------|------|
| VS Code YouTube Channel | [youtube.com/@code](https://www.youtube.com/@code) |
| GitHub YouTube Channel | [youtube.com/@GitHub](https://www.youtube.com/@GitHub) |

### Community
| Resource | Link |
|----------|------|
| VS Code on GitHub | [github.com/microsoft/vscode](https://github.com/microsoft/vscode) |
| Stack Overflow: VS Code | [stackoverflow.com/questions/tagged/vscode](https://stackoverflow.com/questions/tagged/vscode) |
| VS Code Reddit | [reddit.com/r/vscode](https://www.reddit.com/r/vscode/) |
| VS Code on Bluesky | [bsky.app/profile/vscode.dev](https://bsky.app/profile/vscode.dev) |

---

*Last updated: March 2026*
