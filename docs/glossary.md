# Glossary of AI & Developer Tooling Terms

A plain-language reference for terms you'll encounter when building GitHub Copilot agents, working with LLMs, and using modern AI developer tooling.

---

## A

### Agent
A software component that uses an LLM (Large Language Model) to autonomously reason, plan, and take actions to accomplish a goal. Unlike a simple chatbot, an agent can call tools, read files, run code, and make multi-step decisions. In VS Code Copilot Chat, agents are selected from the agents dropdown and are defined as `.agent.md` files.

### `.agent.md` File
The file format used to define custom agents in VS Code Copilot Chat. It's a Markdown file with an optional YAML frontmatter header (defining `description`, `tools`, `model`, `agents`, `handoffs`, etc.) and a body containing the agent's instructions. These files are stored in `.github/agents/` in your workspace or in your VS Code user profile.

### Agent Skills
An open standard ([agentskills.io](https://agentskills.io/)) for packaging reusable AI capabilities as folders containing instructions, scripts, and resources. Skills are loaded on-demand by agents and work across multiple tools, including VS Code, GitHub Copilot CLI, and the GitHub Copilot coding agent.

### AI (Artificial Intelligence)
The broad field of computer science focused on creating systems that can perform tasks typically requiring human intelligence — reasoning, learning, perception, and language understanding.

### API (Application Programming Interface)
A set of rules and protocols that allow software applications to communicate with each other. Copilot agents call external APIs to fetch data, trigger actions, or interact with third-party services.

### Agentic AI
AI systems designed to act autonomously on behalf of the user — planning tasks, calling tools, handling errors, and iterating until a goal is met. Copilot's agent mode is an example of agentic AI.

---

## B

### Breadth-First Search (in AI context)
An exploration strategy where an agent considers multiple approaches at the same level before going deeper. Some agent orchestrators use this to evaluate several tool calls before committing to a path.

---

## C

### Chain-of-Thought (CoT)
A prompting technique where the LLM is encouraged to "think step by step" before answering. This improves reasoning on complex tasks. You may see this in agent system prompts.

### Chat Completion API
The API endpoint (e.g., OpenAI's `/v1/chat/completions`) that takes a list of messages (system, user, assistant) and returns a model-generated response. Copilot Extensions communicate through a similar protocol.

### Context Window
The maximum amount of text (measured in tokens) an LLM can process in a single request. Larger context windows allow agents to "see" more code, conversation history, and tool outputs simultaneously.

### Copilot Extension
A GitHub App that integrates into the Copilot Chat experience. Extensions can provide custom agents (invoked with `@`), slash commands, or skill-based integrations.

### Custom Agent
A user-defined agent in VS Code Copilot Chat, configured via a `.agent.md` file. Custom agents specify a persona (instructions), available tools, language model, and handoffs. They appear in the agents dropdown alongside built-in agents. See also: **`.agent.md` File**.

### CORS (Cross-Origin Resource Sharing)
A browser security mechanism. Relevant when building web-based agent UIs that call your agent's backend API from a different domain.

---

## D

### Deterministic vs Stochastic
**Deterministic** — same input always gives the same output (traditional code).  
**Stochastic** — output varies due to randomness (LLMs with temperature > 0). Agents are inherently stochastic, which is why testing them requires special strategies.

---

## E

### Embedding
A numerical vector representation of text (or images, code, etc.) in a high-dimensional space. Similar items have vectors that are close together. Used in RAG and semantic search to find relevant documents.

### Endpoint
A specific URL path on a server that accepts requests. Your Copilot agent exposes an endpoint (e.g., `POST /agent`) that GitHub calls when a user invokes it.

---

## F

### Few-Shot Prompting
Providing a few examples in the prompt to guide the model's output format and behaviour. E.g., showing 2–3 example question-answer pairs before the real question.

### Fine-Tuning
Training an existing LLM on additional domain-specific data to improve its performance on particular tasks. This is different from prompting, which doesn't change the model's weights.

### Function Calling
The ability of an LLM to output structured JSON describing a function it wants to invoke, rather than (or in addition to) natural language. This is how agents trigger tools. OpenAI, Anthropic, and GitHub Copilot all support function/tool calling.

---

## G

### GitHub App
A first-class integration on GitHub that can respond to webhooks, access APIs, and (for Copilot Extensions) serve as a chat agent. Every Copilot Extension is backed by a GitHub App.

### GitHub Copilot
GitHub's AI-powered coding assistant. It offers code completions, chat conversations, and an extensible agent framework.

### Grounding
Connecting an LLM's responses to real, verifiable data sources (databases, APIs, documents) rather than relying solely on the model's training data. Reduces hallucinations.

---

## H

### Hallucination
When an LLM generates information that sounds plausible but is factually incorrect or fabricated. Agents mitigate this by grounding responses in real data via tool calls.

### Handoff
A mechanism in VS Code custom agents that creates interactive **buttons** at the end of a chat response, allowing the user to transition to another agent with context and a pre-filled prompt. Defined in the `handoffs` section of an `.agent.md` file's YAML frontmatter. Handoffs enable multi-step workflows like Plan → Implement → Test.

### Hooks
Custom shell commands that execute at key lifecycle points during agent sessions in VS Code. Hooks provide deterministic automation that runs regardless of how the agent is prompted — for example, running a linter after every file edit, or blocking dangerous terminal commands.

---

## I

### Inference
The process of running input through a trained model to get a prediction or output. When you ask Copilot a question, the model performs inference to generate the answer.

### Instruction Tuning
A form of fine-tuning where the model is trained on instruction-response pairs, making it better at following user instructions. Most modern chat models (GPT-4, Claude, etc.) are instruction-tuned.

---

## J

### JSON Schema
A vocabulary for annotating and validating JSON documents. Used extensively in tool/function definitions to describe what parameters a tool accepts.

---

## L

### LangChain
A popular open-source framework for building applications powered by LLMs. Provides abstractions for chains, agents, memory, and tool use. Often used as the backbone for complex agent systems.

> 🔗 [langchain.com](https://www.langchain.com/)

### LLM (Large Language Model)
A neural network trained on vast amounts of text data that can generate, understand, and reason about natural language and code. Examples: GPT-4, Claude, Gemini, LLaMA, Codex.

### LoRA (Low-Rank Adaptation)
A technique for efficiently fine-tuning LLMs by training only a small number of additional parameters instead of the full model. Makes fine-tuning accessible with less compute.

---

## M

### MCP (Model Context Protocol)
An open protocol (created by Anthropic) that standardises how LLMs connect to external tools and data sources. Think of it as a **USB-C for AI** — a universal plug that lets any LLM talk to any tool server.

**Key concepts:**
- **MCP Server** — a lightweight process that exposes tools (functions), resources (data), and prompts to an LLM.
- **MCP Client** — the LLM host (e.g., VS Code Copilot, Claude Desktop) that connects to MCP servers.
- **Transport** — how client and server communicate: `stdio` (local subprocess) or `SSE/HTTP` (remote).

```
┌────────────┐     MCP Protocol     ┌────────────────┐
│  LLM Host  │ ◄──────────────────► │  MCP Server    │
│ (Copilot)  │   tools / resources  │ (your tools)   │
└────────────┘                      └────────────────┘
```

**Example:** An MCP server that provides a `search_jira` tool. Once connected, any Copilot agent can call `search_jira` without knowing Jira's API details.

> 🔗 [modelcontextprotocol.io](https://modelcontextprotocol.io/)  
> 🔗 [MCP Specification](https://spec.modelcontextprotocol.io/)  
> 🔗 [GitHub: modelcontextprotocol](https://github.com/modelcontextprotocol)

### Model Temperature
A parameter (typically 0.0–2.0) that controls randomness in LLM outputs. Lower values (e.g., 0.1) produce more focused, deterministic responses; higher values (e.g., 1.0) produce more creative, varied outputs.

### Multi-Modal
Models that can process and generate multiple types of data — text, images, audio, video. GPT-4o and Gemini are multi-modal.

---

## N

### ngrok
A tool that creates a secure tunnel from a public URL to your local machine. Essential for developing Copilot Extensions locally — GitHub needs a public URL to send requests to your agent.

> 🔗 [ngrok.com](https://ngrok.com/)

---

## O

### Orchestration
The process of coordinating multiple agents, tools, or steps to accomplish a complex task. An orchestrator agent might route requests to specialist agents, aggregate results, and handle errors.

---

## P

### Playwright
An open-source **browser automation and end-to-end testing framework** created by Microsoft. It can control Chromium, Firefox, and WebKit browsers programmatically.

**What it does:**
- Automates web browsers (clicking, typing, navigating, screenshotting)
- Runs end-to-end tests for web applications
- Supports multiple languages: JavaScript/TypeScript, Python, Java, C#
- Can run headless (no visible browser) or headed

**Why it matters for AI agents:**
- Agents can use Playwright as a **tool** to browse the web, scrape content, or test web apps
- MCP servers exist that wrap Playwright, giving LLMs the ability to interact with web pages
- Useful for agents that need to verify UI changes, extract data from websites, or automate web workflows

```javascript
// Example: Playwright taking a screenshot
const { chromium } = require('playwright');
const browser = await chromium.launch();
const page = await browser.newPage();
await page.goto('https://github.com');
await page.screenshot({ path: 'github.png' });
await browser.close();
```

> 🔗 [playwright.dev](https://playwright.dev/)  
> 🔗 [GitHub: microsoft/playwright](https://github.com/microsoft/playwright)

### Prompt Engineering
The practice of designing and refining the text instructions (prompts) given to an LLM to get the desired output. System prompts, few-shot examples, and chain-of-thought instructions are all prompt engineering techniques.

### Prompt Injection
A security concern where malicious input tricks an LLM into ignoring its system prompt and following attacker instructions instead. Agents must be designed to guard against this.

---

## R

### RAG (Retrieval-Augmented Generation)
A pattern where an LLM's response is enhanced by first **retrieving** relevant documents from a knowledge base (using embeddings/search) and then **generating** a response grounded in those documents.

```
User query → Search knowledge base → Retrieve relevant docs → Feed to LLM → Generate answer
```

RAG reduces hallucinations and keeps responses up-to-date without retraining the model.

### REST API
An architectural style for web services using standard HTTP methods (GET, POST, PUT, DELETE). Most external services that Copilot agents interact with expose REST APIs.

---

## S

### Semantic Search
Search based on **meaning** rather than exact keyword matching. Uses embeddings to find documents that are conceptually similar to the query, even if they use different words.

### Server-Sent Events (SSE)
A standard for a server to push real-time updates to a client over HTTP. Copilot Extensions use SSE to **stream** agent responses token-by-token, giving users a "typing" effect.

### Skill
In Copilot's architecture, a skill is a specific capability (e.g., "read file", "search web", "run terminal command") that can be invoked by the LLM during a conversation. Skills are similar to tools but are built into the Copilot platform.

### Slash Command
A command prefixed with `/` in Copilot Chat (e.g., `/explain`, `/fix`, `/tests`). Custom slash commands can also be created via **prompt files** (`.prompt.md`).

### Subagent
An agent that is called autonomously by another agent during a conversation, without direct user interaction. Configured via the `agents` property in an `.agent.md` file. The parent agent must also have `agent` in its `tools` list. Subagents are useful for orchestrator agents that delegate tasks to specialists.

### System Prompt
The hidden instruction text sent to the LLM before the user's message. It defines the agent's personality, capabilities, constraints, and behaviour. The system prompt is the primary way you configure an agent's identity.

---

## T

### Token
The basic unit of text that an LLM processes. A token is roughly 3–4 characters of English text, or about ¾ of a word. "Hello world" is 2 tokens. Costs and context limits are measured in tokens.

### Tool / Tool Calling
A function that an LLM can invoke to take action in the real world — reading files, calling APIs, running code, querying databases. The model outputs a structured tool call, and the agent runtime executes it and returns the result.

### Transformer
The neural network architecture behind modern LLMs. Introduced in the 2017 paper "Attention Is All You Need." Uses self-attention mechanisms to process sequences of tokens in parallel.

---

## V

### Vector Database
A database optimised for storing and querying embedding vectors. Used in RAG systems to quickly find documents similar to a query. Examples: Pinecone, Weaviate, Chroma, pgvector.

### VS Code Extension API
The API that allows developers to extend Visual Studio Code with custom features. Copilot Extensions can also leverage the VS Code extension API for deeper IDE integration.

---

## W

### Webhook
An HTTP callback — when an event occurs, a service sends an HTTP POST to a configured URL. GitHub Apps (including Copilot Extensions) receive events via webhooks.

---

## Z

### Zero-Shot Prompting
Asking an LLM to perform a task with no examples — relying entirely on the model's pre-trained knowledge and the instruction in the prompt. Contrast with few-shot prompting.

---

*Last updated: March 2026*
