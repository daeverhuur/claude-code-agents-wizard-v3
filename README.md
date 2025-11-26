# Claude Code Agent Orchestration System v2

A powerful orchestration system for Claude Code optimized for Next.js development. Uses specialized agents with proper model selection, context management, and comprehensive MCP server integration.

## What Is This?

This is a **custom Claude Code orchestration system** that transforms how you build software projects. Claude Code acts as the orchestrator with its 200k context window, managing the big picture while delegating individual tasks to specialized subagents:

| Agent | Model | Purpose |
|-------|-------|---------|
| **Orchestrator** | Your context | Master planner with 200k context, manages todos and delegates |
| **Planner** | Opus | Architecture decisions, complex analysis, strategic planning |
| **Coder** | Sonnet | Implements one todo at a time in isolated context |
| **Tester** | Sonnet | Visual verification using Playwright MCP |
| **Stuck** | Sonnet | Human escalation - no fallbacks allowed |

## Key Features

- **Model Selection Strategy**: Right model for each task (Haiku/Sonnet/Opus)
- **Context Management**: Strategic chunking, efficient context usage
- **No Fallbacks**: When ANY agent hits a problem, you get asked
- **Visual Testing**: Playwright MCP for screenshot-based verification
- **10 MCP Servers**: Pre-configured for Next.js development
- **Architecture Planning**: Opus-powered planner for complex decisions

---

## Quick Start

### Prerequisites

1. **Claude Code CLI** installed ([get it here](https://docs.anthropic.com/en/docs/claude-code))
2. **Node.js v20.19+** (for MCP servers)

### Installation

```bash
# Clone this repository
git clone https://github.com/IncomeStreamSurfer/claude-code-agents-wizard-v2.git
cd claude-code-agents-wizard-v2

# Start Claude Code in this directory
claude
```

The agents and MCP servers are automatically loaded from the `.claude/` directory.

---

## MCP Servers Included

This system includes 10 pre-configured MCP servers optimized for Next.js development:

| Server | Purpose | Config Required |
|--------|---------|-----------------|
| **playwright** | Visual testing, screenshots, browser automation | None |
| **filesystem** | Secure file operations with access controls | None |
| **memory** | Persistent knowledge graph for project context | None |
| **sequential-thinking** | Structured problem-solving for complex tasks | None |
| **next-devtools** | Next.js errors, logs, metadata, server actions | None |
| **fetch** | Web content fetching for LLM usage | None |
| **github** | Repository management, PRs, issues, commits | Token |
| **postgres** | PostgreSQL database integration | Connection string |
| **supabase** | Supabase project management, schema design | URL + Key |
| **figma** | Design-to-code: Figma files, components, tokens | Token |

### Configuring MCP Servers

Edit `.mcp.json` to add your credentials:

```json
{
  "mcpServers": {
    "github": {
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token_here"
      }
    },
    "supabase": {
      "env": {
        "SUPABASE_URL": "https://your-project.supabase.co",
        "SUPABASE_SERVICE_KEY": "your-service-key"
      }
    },
    "figma": {
      "env": {
        "FIGMA_ACCESS_TOKEN": "your-figma-token"
      }
    }
  }
}
```

---

## How It Works

### The Workflow

```
USER: "Build X"
    |
ORCHESTRATOR: Creates detailed todos with TodoWrite
    |
[Complex feature?] --> YES --> PLANNER (Opus): Architecture analysis
    |                              |
    |<-----------------------------+
    |
ORCHESTRATOR: Invokes coder subagent for todo #1
    |
CODER (Sonnet): Implements feature in isolated context
    |
    +---> Problem? --> STUCK: Asks you --> Continue with decision
    |
CODER: Reports completion
    |
ORCHESTRATOR: Invokes tester subagent
    |
TESTER (Sonnet): Playwright screenshots & verification
    |
    +---> Test fails? --> STUCK: Asks you --> Continue with decision
    |
TESTER: Reports success
    |
ORCHESTRATOR: Marks todo complete, moves to next
    |
Repeat until all todos done
```

### Model Selection Strategy

Based on Claude's best practices:

| Task Type | Model | Examples |
|-----------|-------|----------|
| **Quick tasks** | Haiku | Simple edits, formatting, basic queries |
| **Primary development** | Sonnet | Feature implementation, debugging, testing |
| **Complex analysis** | Opus | Architecture planning, design decisions |

**80/20 Rule**: Use Sonnet for most work, reserve Opus for advanced reasoning.

### Context Management

- **Reserve 20%** of context for complex operations
- **Use `/compact`** before hitting limits
- **Use `/clear`** for fresh starts on unrelated tasks
- **Strategic chunking**: Complete components before integration

---

## The Agents

### Orchestrator (CLAUDE.md)
**Your 200k Context Window**

- Creates and maintains todo lists
- Delegates tasks to specialized agents
- Tracks overall progress
- Maintains project state

### Planner Agent (Opus)
**Deep Analysis & Architecture**

- Invoked BEFORE complex features
- Evaluates architectural trade-offs
- Provides implementation order
- Uses Opus for advanced reasoning

### Coder Agent (Sonnet)
**Implementation Specialist**

- Gets ONE specific todo item
- Works in isolated context
- Follows existing patterns
- Escalates ALL problems to stuck agent

### Tester Agent (Sonnet)
**Visual QA Specialist**

- Uses Playwright MCP for screenshots
- Verifies rendered output
- Tests interactions
- Never marks failing tests as passing

### Stuck Agent (Sonnet)
**Human Escalation Point**

- ONLY agent that can ask questions
- Invoked when ANY problem occurs
- Presents clear options
- Returns human's decision

---

## The "No Fallbacks" Rule

**Traditional AI**: Hits error -> tries workaround -> might fail silently

**This system**: Hits error -> asks you -> you decide -> proceeds correctly

Every agent is **hardwired** to invoke the stuck agent rather than use fallbacks.

---

## Example Session

```
You: "Build a landing page with a contact form"

Orchestrator creates todos:
  [ ] Set up HTML structure
  [ ] Create hero section
  [ ] Add contact form with validation
  [ ] Style with CSS
  [ ] Test form submission

Orchestrator invokes coder(todo #1: "Set up HTML structure")

Coder (isolated context): Creates index.html
Coder: Reports completion

Orchestrator invokes tester("Verify HTML structure loads")

Tester (isolated context): Takes screenshot with Playwright
Tester: Reports success

Orchestrator: Marks todo #1 complete

Orchestrator invokes coder(todo #2: "Create hero section")

Coder: ERROR - image file not found
Coder: Invokes stuck agent

Stuck: Asks YOU:
  "Hero image 'hero.jpg' not found. How to proceed?"
  Options:
  - Use placeholder image
  - Download from Unsplash
  - Skip image for now

You choose: "Download from Unsplash"

Stuck: Returns decision to coder
Coder: Proceeds with Unsplash download
Coder: Reports completion

... continues until all todos done
```

---

## Repository Structure

```
.
├── .claude/
│   ├── CLAUDE.md              # Orchestrator instructions
│   └── agents/
│       ├── coder.md           # Coder agent (Sonnet)
│       ├── tester.md          # Tester agent (Sonnet)
│       ├── stuck.md           # Stuck agent (Sonnet)
│       └── planner.md         # Planner agent (Opus)
├── .mcp.json                   # MCP server configurations
├── .gitignore
└── README.md
```

---

## Best Practices

1. **Let Claude plan** - It will create comprehensive todo lists
2. **Review screenshots** - Tester provides visual proof
3. **Make decisions when asked** - Stuck agent needs your guidance
4. **Don't interrupt the flow** - Let agents complete their work
5. **Check context usage** - Use `/context` to monitor
6. **Use planner for architecture** - Don't skip design decisions

---

## Pro Tips

- Use `/agents` to see available subagents
- Use `/compact` before complex operations
- Use `/context` to debug context usage
- Screenshots from tester are saved for review
- Each agent has specific MCP server access
- Planner uses Opus - use it for hard decisions

---

## Optimizations Applied

Based on [Claude Code best practices](https://claudelog.com/claude-code-limits/):

1. **Model Selection**: Haiku/Sonnet/Opus hierarchy by task complexity
2. **Context Management**: 80/20 rule, strategic chunking
3. **Turn Limits**: Appropriate limits per task type
4. **Reset Cycle Alignment**: Schedule intensive work near resets
5. **Task Segmentation**: Complete components before integration

---

## Resources

- **[Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)**
- **[MCP Server Registry](https://github.com/modelcontextprotocol/servers)**
- **[Figma MCP Server](https://www.figma.com/blog/introducing-figma-mcp-server/)**
- **[Supabase MCP Server](https://supabase.com/docs/guides/getting-started/mcp)**
- **[Next.js MCP Server](https://github.com/vercel/next-devtools-mcp)**

---

## Contributing

Feel free to:
- Add new specialized agents
- Improve agent prompts
- Add MCP server configurations
- Share optimizations

---

## License

MIT - Use it, modify it, share it!

---

**Ready to build something amazing?** Run `claude` in this directory and describe your project!
