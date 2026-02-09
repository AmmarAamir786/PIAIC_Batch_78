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

**Last Updated:** February 7, 2026
