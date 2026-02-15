# Course Progress - PIAIC Batch 78

## Overview

This document tracks what we have covered in class so far. It serves as a reference for students to review key concepts, tools, and resources we've learned about.

---

## Topics Covered

### 1. Digital FTE (Full-Time Equivalent) Concept

We explored the concept of Digital FTE and how AI agents are transforming the workplace.

**Resource:**

- [Digital FTE Presentation
  ](https://docs.google.com/presentation/d/1UGvCUk1-O8m5i-aTWQNxzg8EXoKzPa8fgcwfNh8vRjQ/edit)

---

### 2. Claude Code and Claude Code Router

Learned how to set up and use Claude Code, an AI-powered coding assistant, along with the Claude Code Router for enhanced functionality.

**Setup Guide:**

- [Claude Code Setup Repository](https://github.com/AmmarAamir786/claude_code_setup)

**What We Learned:**

- Installing and configuring Claude Code
- Setting up Claude Code Router
- Basic usage and commands
- Best practices for working with Claude Code

---

### 3. CLAUDE.MD File

Explored the purpose and usage of `CLAUDE.MD` files in projects.

**Key Concepts:**

- What is CLAUDE.MD and why it's important
- How to structure CLAUDE.MD for your project
- Providing context to Claude for better assistance
- Best practices for writing effective CLAUDE.MD files

---

### 4. Claude Code Skills

Introduction to Claude Code Skills - pre-built capabilities that extend Claude's functionality for specific tasks.

**What We Learned:**

- Understanding what skills are and how they work
- Where to find skills
- How skills enhance Claude's capabilities
- The `.claude/skills` folder structure

---

### 5. Downloading and Using Existing Skills

Learned how to download and implement pre-built skills from the community.

**Practical Example:**

- Used the **Resume Writer Skill** from [MCP Market](https://mcpmarket.com/tools/skills/resume-builder)

**Skills Covered:**

- How to browse and find skills on MCP Market
- Downloading skills to your project
- Installing skills in the `.claude/skills` folder
- Using skills with Claude Code
- Testing and validating skill functionality

---

### 6. Creating Custom Skills

Learned how to create our own custom skills tailored to specific needs.

**Resource:**

- [Claude Code Skills Lab](https://github.com/panaversity/claude-code-skills-lab)

**Key Skill Used:**

- **skill-creator-skill** - A meta-skill that helps create new skills

---

## Hands-On Practice

### Projects/Exercises Completed:

1. ✅ Set up Claude Code and Claude Code Router
2. ✅ Created CLAUDE.MD file for our project
3. ✅ Downloaded and used the Resume Writer skill
4. ✅ Created a custom skill using skill-creator-skill
5. ✅ Tested and refined our custom skills

---

## Important Resources

### Documentation & Guides

- [Digital FTE Presentation](https://docs.google.com/presentation/d/1UGvCUk1-O8m5i-aTWQNxzg8EXoKzPa8fgcwfNh8vRjQ/edit)
- [Claude Code Setup Guide](https://github.com/AmmarAamir786/claude_code_setup)
- [Claude Code Skills Lab](https://github.com/panaversity/claude-code-skills-lab)

### Tools & Platforms

- [MCP Market](https://mcpmarket.com/) - Marketplace for Claude skills and tools
- [Resume Builder Skill](https://mcpmarket.com/tools/skills/resume-builder)

---

## Skills We've Used

| Skill Name      | Source                 | Purpose                              |
| --------------- | ---------------------- | ------------------------------------ |
| Resume Writer   | MCP Market             | Generate professional resumes        |
| Skill Creator   | Panaversity Skills Lab | Create custom skills                 |
| Skill Validator | Panaversity Skills Lab | Validate skill structure and quality |

---

### 7. Claude Code Context Management & MCP Servers

Deep dive into how Claude Code manages context and integrates with MCP (Model Context Protocol) servers for enhanced capabilities.

**Context Management Topics Covered:**

- Understanding `/context` command to view context usage
- What takes up context by default (skills, files, conversation history)
- Auto compact buffer - how it optimizes context usage
- Skills structure and their context footprint
- Keyboard shortcuts with `?` command
- How to add files as context in prompts
- Difference between `/clear` and `/compact` commands
- Pressing double escape to restore a checkpoint

**MCP Servers Deep Dive:**

- What are MCP (Model Context Protocol) servers
- How to connect existing MCP servers with Claude Code
- Context usage impact of MCP servers
- **Playwright MCP Server**: Connected and explored use cases for web automation and testing
- **Claude Extension on Chrome**: Learned about Playwright's alternative approach
- **Context7 MCP Server**: Integrated context7, understood the problem it solves (semantic search), how to use it, and how to obtain API keys for increased search limits

**Hands-On Setup:**

- Connected Playwright MCP server for web interaction capabilities
- Connected Context7 MCP server for enhanced semantic search functionality
- Explored context budgeting across multiple MCP integrations

---

### 8. Claude & Agent Orchestration

Explored advanced topics in agent orchestration, configuration files, and understanding how main and sub-agents work together.

**Markdown & VS Code:**

- Markdown files use `.md` extension (claude.md, agents.md, skills.md)
- Preview markdown files in VS Code using preview icon or markdown viewer extension
- Markdown is used for instructions, skills, agent configuration, and clean formatting

**Configuration Files:**

- **CLAUDE.md**: Defines Claude Code's behavior
  - Project-specific instructions for Claude Code
  - Controls how Claude responds and behaves
  - Can include directory structure explanations
  - Can reference other files using `@Agents.md` syntax
- **AGENTS.md**: Shared configuration for multiple coding agents
  - Can be referenced by Claude Code using `@Agents.md` in CLAUDE.md
  - Other coding agents such as qwen cli, gemini cli and copilot can also reference this file
  - Useful for maintaining consistent instructions across different AI assistants
  - Helps document agent roles and responsibilities in multi-agent projects

**Agent Orchestration Architecture:**

**Main Agent:**

- Receives user prompts
- Handles user queries
- Breaks tasks into smaller parts
- Delegates work to sub-agents
- Combines final output from sub-agents
- Has its own context window

**Sub-Agents:**

- Support the main agent
- Handle specific tasks (research, writing, coding, debugging)
- Work independently in their own context window
- Generate summaries and return results to main agent
- Field-specific capabilities

**Orchestration Flow:**

1. User submits query
2. Main agent analyzes task
3. Decides which sub-agent to delegate to
4. Sub-agents perform assigned tasks in their own context window
5. Sub-agents summarize output and return to main agent
6. Main agent delivers final output

**Creating Custom Sub-Agents:**

- Use `/agents` command in your project folder to create custom sub-agents
- Assign specific roles, examples, and tasks to each sub-agent
- Each agent can be specialized for different fields or purposes
- Custom agents are stored in `.claude/agents` folder
- Sub-agents work independently with their own context windows
- Main agent invokes sub-agents using when needed

**Context Management Best Practices:**

- Main agent has its own context window
- Each sub-agent has its own separate context window
- Avoid giving unnecessary data to the main agent
- Main agent will automatically use explore sub-agent to search through project directories efficiently
- Sub-agents report findings back without polluting main agent's context

**Configuration Types:**

- **Global configuration**: Applies to all projects
- **Personal configuration**: Project-specific settings

**Hooks:**

Hooks are shell commands that execute automatically in response to specific events during Claude Code's operation (such as tool calls).

**What Hooks Enable:**

- Run custom scripts when certain events occur
- Execute validation checks automatically
- Trigger build processes or tests
- Integrate with external tools and workflows
- Automate repetitive tasks based on Claude's actions

**Hooks vs Prompts:**

| Prompts        | Hooks                  |
| -------------- | ---------------------- |
| Text input     | Shell commands         |
| Manual         | Automatic/event-driven |
| Static         | Dynamic/reactive       |
| User-initiated | Event-triggered        |

**Key Takeaways:**

- Markdown is essential for agent configuration
- `claude.md` is for Claude Code; can reference shared files using `@` syntax
- `agents.md` can be shared across multiple coding agents when referenced properly
- Custom sub-agents can be created with `/agents` command and stored in `.claude/agents`
- Sub-agents simplify complex tasks through delegation and parallel processing
- Proper context management prevents information overload and helps Claude focus only on the problem at hand
- Hooks enable event-driven automation through shell commands

---

### 9. Claude Code Stop Hook, Plugins & Advanced Workflows

Deep dive into Claude Code's stop hook functionality, plugins ecosystem, and advanced development workflows.

**Stop Hook in Detail:**

- **What is Stop Hook**: A special hook that executes shell commands automatically before Claude Code stops or exits
- **Use Cases**:
  - Running cleanup scripts
- **Configuration**: Defined in Claude Code settings ".claude/settings.json"
- **Event Trigger**: Fires when Claude Code session ends

**Plugins:**

- **What are Plugins**: Extensions that add additional functionality to Claude Code
- **Plugin Management**: Install, enable, disable, and configure plugins

**Anthropic's Marketplace:**

- **Overview**: Official marketplace for Claude Code plugins, skills, and MCP servers
- **What's Available**:
  - Verified plugins from Anthropic
  - Community-contributed tools
  - Pre-built skills and templates
  - MCP server integrations
- **Benefits**: Quality-assured, well-documented, and community-supported resources

**Ralph Wiggum Loop in Detail:**

**What is Ralph Wiggum Loop**: A Claude Code plugin that enables **autonomous iteration**—Claude runs a task, checks results, identifies problems, fixes them, and repeats until completion criteria are met, all without your intervention

- **The Problem It Solves**: **Iteration fatigue**—manually running commands, copying errors, pasting back to Claude, and repeating this cycle dozens of times

  - Example: Fixing 47 linting errors requires copying output and waiting for Claude 10+ times
  - Hidden costs: waiting time, context switching, error transcription mistakes
- **How It Works**:

  - Uses **Stop hook** to intercept Claude's normal exit behavior
  - When Claude tries to stop, the hook checks if completion promise is met
  - If not complete: reinjects prompt asking Claude to verify, fix, and continue
  - If complete or max iterations reached: allows Claude to stop
  - Claude remembers conversation history, creating a **self-correcting loop**
- **Architecture**:

  - **Stop Hook**: Intercepts Claude's exit to reinject continuation prompts
  - **Completion Promise**: Exact text that signals "we're done" (e.g., "0 problems" or custom marker)
  - **Max Iterations**: Safety limit preventing infinite loops and controlling costs
  - **Loop Prompt**: Template asking Claude to verify results and continue
- **Best Practice Usage Pattern** (Embedded Promise - Recommended):

  ```bash
  /ralph-loop "Task description:
  - Requirement 1
  - Requirement 2
  Output <promise>COMPLETION_MARKER</promise> when done." \
  --max-iterations 20 \
  --completion-promise "COMPLETION_MARKER"
  ```
- **Good Use Cases** (10+ iterations, objective completion):

  - Framework upgrades (React v16→v19, Next.js 14→15)
  - Test-driven refactoring (fix until all tests pass)
  - Linting/type error resolution
  - Deployment debugging (iterate until health check passes)
- **Poor Use Cases**:

  - Tasks requiring human judgment (strategy, aesthetics, priorities)
  - Exploratory research (no clear completion criteria)
  - Creative work (subjective quality assessment)
  - Multi-goal tasks ("fix bugs AND add features")
  - Tasks requiring external input (API keys, approvals)
- **Safety and Cost Management**:

  - Real-world costs: $50-100 for 14-hour sessions, $30-40 for 30 iterations
  - **Always** set `--max-iterations` limit
  - Use version control checkpoints before starting
  - Monitor progress every 15-30 minutes
  - Cancel with `/cancel-ralph` if same error repeats 3+ times
  - Review results before merging—automation doesn't replace judgment
- **Golden Rule**: Use Ralph Loop when success is **objective, verifiable, and deterministic**—measurable by tools, not human judgment

**Creator's Workflow (Boris Cherny - Claude Code Creator at Anthropic):**

- **Fundamental Principle**: Context window is the constraint—all practices manage context to prevent degradation
- **Core Practices**:
  - **Parallel Sessions** (3-20): "Single biggest productivity unlock"—run multiple isolated sessions for different tasks using separate directories/tabs
  - **Plan Mode First**: Always use Plan Mode (Shift+Tab twice) for non-trivial tasks; iterate on plan until solid, then auto-accept execution
  - **Claude-Reviews-Claude**: One session writes plan, second session reviews critically with fresh context to catch blind spots
  - **Self-Writing CLAUDE.md**: After every correction say "Update your CLAUDE.md so you don't make that mistake again"—Claude writes better rules for itself
  - **Session-End Review Skill**: Build `/session-review` or `/techdebt` skill, run at end of every session while context is fresh
  - **Skills Portfolio**: "If you do something more than once a day, turn it into a skill"—build portable, reusable skills across projects
  - **Subagents for Investigation**: Use subagents to explore in separate context windows, report back summaries without cluttering main session

**Key Takeaways:**

- Stop hooks provide automation opportunities at session end
- Plugins extend Claude Code's core functionality beyond skills
- Anthropic's marketplace offers curated, quality resources
- Ralph Wiggum Loop enables autonomous iteration through Stop hook mechanism
- Creator's Workflow provides production-grade practices for managing context and building with Claude
- Agent Teams coordinate multiple independent Claude sessions for complex multi-perspective work
- Proper planning and validation prevent common pitfalls

---

### 10. Agent Teams

Coordinating multiple independent Claude Code sessions that communicate directly with each other through shared task lists and messaging.

**What It Is:**

- Multiple independent Claude sessions with separate context windows
- Direct messaging between teammates (not just to lead)
- Self-coordinate through shared task list with dependency chains

**vs Subagents:**

- **Subagents**: Fire-and-forget workers that report back to single caller
- **Agent Teams**: Fully independent sessions that message each other and discuss findings

**Setup:**

Enable experimental feature in settings:

```json
{ "env": { "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1" } }
```

**When to Use:**

- **Use Teams**: Multi-angle investigations, competing hypotheses, cross-functional coordination where teammates need to discuss findings
- **Use Subagents**: Single reports, simple summaries, tasks that just report back

**Cost Consideration:**

- 3-5x more tokens than single agent (each teammate has full context window)
- Use strongest model for lead (synthesis), efficient models for teammates (research)

**Common Patterns:**

1. **Parallel Investigation**: Multiple specialists analyze same question from different angles, share findings
2. **Pipeline Build**: Sequential dependencies where each stage feeds the next
3. **Competing Hypotheses**: Investigators actively debate each other's theories to prevent anchoring bias

**Navigation:**

- Shift+Up/Down to switch between teammates
- Ctrl+T to view shared task list
- Direct message by selecting teammate and typing

---

## Next Steps

- Continue practicing with custom skills
- Validate our skills using skill-validator
- Explore more skills from MCP Market
- Build domain-specific skills for real-world use cases
- Share and collaborate on skills with classmates

---

## Notes

- Always test your skills thoroughly before using them in production
- Keep your skills well-documented for future reference
- Contribute to the community by sharing useful skills
- Stay updated with new skills and features in the Claude ecosystem

---

**Last Updated:** February 15, 2026
