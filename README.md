# Claude Code Plugin Marketplace

A collection of professional Claude Code plugins for enhanced development workflow.

## Available Plugins

### 1. Code Review Plugin

Comprehensive code review with security analysis, quality checks, and PR review capabilities.

**Features:**
- 🔒 Security analysis (OWASP Top 10)
- ✅ Code quality checks for Go, Python, JavaScript/TypeScript
- ⚡ Performance analysis
- 📝 GitHub PR review integration
- 🤖 3 specialized agents (code-reviewer, security-expert, performance-analyzer)
- 🎯 2 autonomous skills (pr-reviewer, code-quality)

**Commands:**
- `/review` - Review code changes or GitHub PR
- `/review --security-only` - Focus on security
- `/review --strict` - Detailed analysis

**Supported Languages:** Go, Python, JavaScript, TypeScript

[→ Documentation](./code-review-plugin/README.md)

---

### 2. Taskmaster Plugin

Complete project management with Taskmaster MCP integration for PRD creation, task decomposition, and workflow automation.

**Features:**
- 📝 PRD (Product Requirements Document) creation and analysis
- 🔄 AI-powered task decomposition
- 🎯 Smart workflow automation with streamlined commands
- 📊 Progress tracking and status management
- 🤖 3 specialized agents with color-coding (prd-architect 🔵, task-decomposer 🟢, task-executor 🟡)
- 🎯 2 autonomous skills (prd-analyzer, task-manager)
- ✨ Simplified 4-command interface for maximum efficiency

**Commands:**
- `/init-taskmaster` - Initialize Taskmaster in project
- `/prd` - Manage PRDs (3 modes):
  - No params: Show PRD overview with task statistics
  - File path: Parse PRD, auto-create tag, analyze and expand tasks
  - Text description: Create PRD with AI, review, then parse
- `/run` - Work on tasks (2 modes):
  - No params: Show overview and auto-start next available task
  - With ID: Execute specific task with step-by-step guidance
- `/next` - Find and start next available task

**Agent Color Coding:**
- 🔵 **Blue** - PRD Architect (analysis, PRD creation)
- 🟢 **Green** - Task Decomposer (task breakdown, dependencies)
- 🟡 **Yellow** - Task Executor (step-by-step execution)

**MCP Integration:** Taskmaster AI (npx -y task-master-ai)

[→ Documentation](./taskmaster-plugin/README.md)

---

## Installation

### Prerequisites

- Claude Code installed
- GitHub CLI (for Code Review Plugin): `brew install gh`

### Quick Start

**Add the marketplace in Claude Code:**

```bash
/plugin marketplace add katalabut/claude-code-marketplace
```

That's it! All plugins are now available.

**Restart Claude Code** to load the plugins.

### Plugin-Specific Setup

#### Code Review Plugin

```bash
# Authenticate GitHub CLI (for PR reviews)
gh auth login

# Ready to use!
/review
```

#### Taskmaster Plugin

```bash
# Set Anthropic API key (required for MCP integration)
export ANTHROPIC_API_KEY="sk-ant-your-key"

# Add to your shell profile for persistence (~/.zshrc or ~/.bashrc)
echo 'export ANTHROPIC_API_KEY="sk-ant-your-key"' >> ~/.zshrc

# Restart Claude Code to apply

# Initialize in your project
cd your-project
/init-taskmaster

# Create PRD from description
/prd Build authentication system with OAuth 2.0 and JWT

# Start working
/next
```

---

## Plugin Comparison

| Feature | Code Review | Taskmaster |
|---------|-------------|------------|
| **Primary Purpose** | Code quality & security | Project management |
| **Commands** | 1 (`/review`) | 4 (`/init-taskmaster`, `/prd`, `/run`, `/next`) |
| **Agents** | 3 (review, security, performance) | 3 (PRD 🔵, decompose 🟢, execute 🟡) |
| **Skills** | 2 (PR review, code quality) | 2 (PRD analyzer, task manager) |
| **MCP Integration** | No | Yes (Taskmaster AI) |
| **External Tools** | `gh`, `git` | Taskmaster MCP |
| **Supported Languages** | Go, Python, JS/TS | Language agnostic |
| **Use Case** | Code review, security audit | PRD creation, task tracking |

---

## Usage Examples

### Code Review Workflow

```bash
# Review local changes
/review

# Review GitHub PR
/review https://github.com/owner/repo/pull/123

# Security-focused review
/review --security-only

# Detailed review before merge
/review --strict
```

### Project Management Workflow

```bash
# 1. Initialize Taskmaster
/init-taskmaster

# 2. Create PRD from description (AI-powered)
/prd Build authentication system with OAuth 2.0, JWT, and MFA support

# AI creates comprehensive PRD → Review → Approve → Auto-parsed!

# 3. Start working (auto-finds next task)
/next
# 🟡 Task Executor guides you through implementation

# 4. Or check overview and start next
/run
# Shows overview → Auto-starts next available task

# 5. Work on specific task
/run 1.2.1
# 🟡 Step-by-step guidance through subtasks

# 6. Continue to next task
/next
# Repeat until all done!

# 7. Check PRD status anytime
/prd
# Shows all PRDs with task statistics
```

### Combined Workflow

Use both plugins together for complete development workflow:

```bash
# 1. Plan with Taskmaster (AI creates PRD)
/prd Add user profile feature with avatar upload and bio

# 2. Start first task (auto-guided)
/next
# 🟡 Task Executor: guides through implementation

# 3. Review code with Code Review Plugin
/review

# 4. Fix any issues found, continue
/next
# Repeat cycle: implement → review → next
```

---

## Architecture

### Repository Structure

```
claude-code-plugins/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace registry
├── code-review-plugin/
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── commands/
│   │   └── review.md
│   ├── agents/
│   │   ├── code-reviewer.md
│   │   ├── security-expert.md
│   │   └── performance-analyzer.md
│   ├── skills/
│   │   ├── pr-reviewer/
│   │   └── code-quality/
│   ├── README.md
│   └── TOOL-PERMISSIONS.md
├── taskmaster-plugin/
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── .mcp.json                 # MCP Taskmaster config
│   ├── commands/
│   │   ├── init-taskmaster.md    # Initialize project
│   │   ├── prd.md                # PRD management (3 modes)
│   │   ├── run.md                # Task execution (2 modes)
│   │   └── next.md               # Find next task
│   ├── agents/
│   │   ├── prd-architect.md      # 🔵 Blue - PRD creation
│   │   ├── task-decomposer.md    # 🟢 Green - Task breakdown
│   │   └── task-executor.md      # 🟡 Yellow - Execution guide
│   ├── skills/
│   │   ├── prd-analyzer/         # Auto PRD analysis
│   │   └── task-manager/         # Auto task suggestions
│   ├── templates/
│   │   ├── prd-template.md
│   │   └── task-structure.md
│   ├── README.md
│   └── TOOL-PERMISSIONS.md
└── README.md                      # This file
```

### Plugin Components

Each plugin consists of:

**Commands** - User-invoked slash commands
- Markdown files with YAML frontmatter
- Define allowed tools
- One-shot interactions

**Agents** - AI-powered specialized assistants
- Automatically invoked based on context
- Multi-turn conversations
- Can use subsets of tools
- Different AI models (Sonnet, Haiku)

**Skills** - Autonomous capabilities
- Automatically activate when relevant
- Background intelligence
- Progressive loading of context

---

## Development

### Creating a New Plugin

1. **Create plugin directory:**
   ```bash
   mkdir my-plugin
   mkdir my-plugin/.claude-plugin
   ```

2. **Create plugin.json:**
   ```json
   {
     "name": "my-plugin",
     "version": "1.0.0",
     "description": "My plugin description",
     "author": {
       "name": "Your Name"
     }
   }
   ```

3. **Add to marketplace.json:**
   ```json
   {
     "plugins": [
       {
         "name": "my-plugin",
         "source": "./my-plugin",
         "description": "My plugin description"
       }
     ]
   }
   ```

4. **Create plugin components:**
   - `commands/` - Slash commands
   - `agents/` - Specialized agents
   - `skills/` - Autonomous skills
   - `README.md` - Documentation

5. **Test and commit**

### Plugin Best Practices

1. **Use clear, descriptive names** for commands, agents, and skills
2. **Document thoroughly** - README, examples, troubleshooting
3. **Specify tool permissions** explicitly in frontmatter
4. **Keep commands concise** - under 200 tokens if possible
5. **Provide examples** in documentation
6. **Test with real scenarios** before publishing
7. **Version semantically** - follow semver (1.0.0)

---

## Troubleshooting

### Plugins Not Appearing

```bash
# Check plugin installation
ls ~/.claude/plugins/claude-code-marketplace/

# Verify marketplace.json
cat .claude-plugin/marketplace.json

# Restart Claude Code
```

### Commands Not Working

```bash
# Check command definition
cat plugin-name/commands/command-name.md

# Verify tool permissions in frontmatter
# Restart Claude Code
```

### MCP Integration Issues (Taskmaster)

```bash
# Check MCP configuration
cat taskmaster-plugin/.mcp.json

# Verify API key is set
echo $ANTHROPIC_API_KEY

# Check MCP server status in Claude Code
/mcp

# Restart Claude Code to reload MCP servers
```

### GitHub CLI Issues (Code Review)

```bash
# Install gh CLI
brew install gh

# Authenticate
gh auth login

# Test
gh repo view owner/repo
```

---

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

### Contribution Guidelines

- Follow existing plugin structure
- Include comprehensive documentation
- Add examples and use cases
- Test with real-world scenarios
- Update marketplace.json
- Follow Claude Code plugin best practices

---

## Resources

### Official Documentation
- **Claude Code**: https://code.claude.com/docs/
- **Plugins Guide**: https://code.claude.com/docs/en/plugins-reference
- **MCP Protocol**: https://modelcontextprotocol.io/

### External Tools
- **GitHub CLI**: https://cli.github.com/
- **Taskmaster AI**: https://github.com/eyaltoledano/claude-task-master
- **MCP Servers**: https://github.com/modelcontextprotocol/servers

### Community
- **Claude Code GitHub**: https://github.com/anthropics/claude-code
- **Report Issues**: https://github.com/anthropics/claude-code/issues

---

## License

MIT License

## Author

katalabut

---

## Changelog

### v1.1.0 (2025-01-08)

**Taskmaster Plugin Refactor**

- 🎯 **Streamlined Commands**: Reduced from 7 to 4 essential commands
  - `/init-taskmaster` - Initialize Taskmaster in project
  - `/prd` - Multi-mode PRD management (3 modes in one command)
  - `/run` - Dual-mode task execution (overview + specific task)
  - `/next` - Smart task finder and auto-starter
  - Removed: `/current`, `/tasks`, `/expand` (functionality integrated)

- 🎨 **Color-Coded Agents**: Visual agent activity markers
  - 🔵 Blue - PRD Architect (PRD creation and analysis)
  - 🟢 Green - Task Decomposer (task breakdown and dependencies)
  - 🟡 Yellow - Task Executor (step-by-step implementation)

- ✨ **Enhanced Workflows**:
  - `/prd` with no params shows overview of all PRDs + statistics
  - `/run` with no params shows overview + auto-starts next task
  - Auto-expansion of complex tasks
  - Seamless agent integration throughout workflow

### v1.0.0 (2025-01-07)

**Initial Release**

Added two professional plugins:

1. **Code Review Plugin**
   - Security analysis with OWASP Top 10
   - Code quality checks for Go, Python, JS/TS
   - Performance analysis
   - GitHub PR review integration
   - 3 agents, 2 skills

2. **Taskmaster Plugin**
   - MCP Taskmaster integration
   - PRD creation and management
   - AI-powered task decomposition
   - Workflow automation
   - 7 commands, 3 agents, 2 skills

---

**Built with Claude Code** - Enhancing development workflow through AI-powered plugins

