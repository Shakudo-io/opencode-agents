# OpenCode Agents

A collection of custom agents for [OpenCode](https://opencode.ai) - the powerful AI coding CLI.

## Available Agents

| Agent | Description |
|-------|-------------|
| [zellij](./agents/zellij/) | Zellij session controller - navigate tabs, execute commands in other panes, and retrieve information from the current Zellij session |

---

### Zellij Agent

**Stop copy-pasting terminal output to your AI agent.**

When vibe coding with OpenCode inside [Zellij](https://zellij.dev), you typically have multiple tabs open - one running your dev server, another with logs, maybe one running tests. Every time something breaks, you'd manually copy error output and paste it into your conversation.

The **zellij agent** eliminates this friction. Just tell it to check the tab:

```
@zellij check what's happening in my "server" tab
@zellij run "npm test" in the tests tab and tell me if it passed  
@zellij grab the last error from the logs tab
```

The agent will:
1. Navigate to the target tab
2. Read the screen content or execute commands
3. Return to your original tab
4. Report what it found

No more context switching. No more copy-paste. Just vibe.

---

## Installation

### For Humans

#### Option 1: Install a Single Agent (Recommended)

Copy the agent file to your OpenCode config directory:

```bash
# Create the agent directory if it doesn't exist
mkdir -p ~/.config/opencode/agent

# Download and install the zellij agent
curl -fsSL https://raw.githubusercontent.com/Shakudo-io/opencode-agents/main/agents/zellij/agent.md \
  -o ~/.config/opencode/agent/zellij.md
```

#### Option 2: Install All Agents

Clone the repo and symlink or copy all agents:

```bash
# Clone the repository
git clone https://github.com/Shakudo-io/opencode-agents.git ~/.opencode-agents

# Create config directory
mkdir -p ~/.config/opencode/agent

# Copy all agents
for agent_dir in ~/.opencode-agents/agents/*/; do
  agent_name=$(basename "$agent_dir")
  cp "$agent_dir/agent.md" ~/.config/opencode/agent/$agent_name.md
done
```

#### Option 3: Project-Specific Installation

For project-specific agents, copy to your project's `.opencode/agent/` directory:

```bash
mkdir -p .opencode/agent
curl -fsSL https://raw.githubusercontent.com/Shakudo-io/opencode-agents/main/agents/zellij/agent.md \
  -o .opencode/agent/zellij.md
```

### For AI Agents

If you are an AI agent helping a user install these agents, use the following commands:

```bash
# Install zellij agent globally
mkdir -p ~/.config/opencode/agent && \
curl -fsSL https://raw.githubusercontent.com/Shakudo-io/opencode-agents/main/agents/zellij/agent.md \
  -o ~/.config/opencode/agent/zellij.md

# Verify installation
ls -la ~/.config/opencode/agent/
```

To install for a specific project:

```bash
# Install zellij agent to current project
mkdir -p .opencode/agent && \
curl -fsSL https://raw.githubusercontent.com/Shakudo-io/opencode-agents/main/agents/zellij/agent.md \
  -o .opencode/agent/zellij.md
```

**Note**: After installation, the user must restart OpenCode for the agent to be available.

## Usage

Once installed, agents can be invoked in OpenCode:

1. **Via @mention**: `@zellij list all my tabs`
2. **Via agent selection**: Select the agent from the agent picker in the TUI
3. **Via CLI flag**: `opencode --agent zellij`

## Agent File Format

Each agent is defined as a Markdown file with YAML frontmatter:

```yaml
---
name: agent-name
description: Brief description of what the agent does
tools:
  bash: true
  read: true
  write: false
  edit: false
  glob: true
  grep: true
---

# Agent Name

System prompt and instructions for the agent...
```

### Frontmatter Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Unique identifier for the agent |
| `description` | string | Brief description shown in agent list |
| `tools` | object | Map of tool names to boolean (enabled/disabled) |
| `model` | string | (Optional) Specific model to use, e.g., `anthropic/claude-sonnet-4-20250514` |
| `mode` | string | (Optional) `primary` or `subagent` |

## Contributing

We welcome contributions! To add a new agent:

1. Fork this repository
2. Create a new directory under `agents/` with your agent name
3. Add an `agent.md` file following the format above
4. Update this README to include your agent in the table
5. Submit a pull request

### Guidelines

- Agent names should be lowercase, using hyphens for multi-word names
- Include comprehensive documentation in the agent's system prompt
- Test your agent thoroughly before submitting
- Keep tool permissions minimal - only enable what's necessary

## License

MIT License - see [LICENSE](./LICENSE) for details.
