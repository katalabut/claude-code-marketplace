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
- 🎯 Smart workflow automation with `/next`, `/run`, `/current`
- 📊 Progress tracking and status management
- 🤖 3 specialized agents (prd-architect, task-decomposer, task-executor)
- 🎯 2 autonomous skills (prd-analyzer, task-manager)

**Commands:**
- `/init-project` - Initialize Taskmaster
- `/prd create "Description"` - Create PRD
- `/prd parse file.md` - Parse PRD into tasks
- `/next` - Get next available task
- `/run [task-id]` - Execute task with guidance
- `/current` - Show in-progress work
- `/tasks` - List and filter tasks
- `/expand [task-id]` - Break down complex tasks

**MCP Integration:** Taskmaster AI (npx -y task-master-ai)

[→ Documentation](./taskmaster-plugin/README.md)

---

## Installation

### Prerequisites

- Claude Code installed
- Git

### Install Plugins

1. **Clone the marketplace:**
   ```bash
   cd ~/.claude/plugins
   git clone https://github.com/katalabut/claude-code-marketplace.git
   ```

2. **Restart Claude Code**

3. **Plugins will be automatically available**

### Plugin-Specific Setup

#### Code Review Plugin
```bash
# Install GitHub CLI (for PR reviews)
brew install gh
gh auth login

# Ready to use!
/review
```

#### Taskmaster Plugin
```bash
# Set API key (required for MCP)
export ANTHROPIC_API_KEY="sk-ant-your-key"

# Initialize in your project
cd your-project
/init-project

# Start using
/prd create "Your feature description"
```

---

## Plugin Comparison

| Feature | Code Review | Taskmaster |
|---------|-------------|------------|
| **Primary Purpose** | Code quality & security | Project management |
| **Commands** | 1 (`/review`) | 7 (`/init-project`, `/prd`, `/next`, etc.) |
| **Agents** | 3 (review, security, performance) | 3 (PRD, decompose, execute) |
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
/init-project

# 2. Create Product Requirements Document
/prd create "Build authentication system with OAuth and JWT"

# 3. Parse PRD into tasks
/prd parse docs/prd/authentication-system-prd.md

# 4. Start working on next task
/next

# 5. Execute the task
/run 1.1.1

# 6. Track progress
/current

# 7. Continue to next task
/next
```

### Combined Workflow

Use both plugins together for complete development workflow:

```bash
# 1. Plan with Taskmaster
/prd create "Add user profile feature"
/prd parse docs/prd/user-profile-prd.md
/next

# 2. Implement the task
/run 1.1.1
# ... write code ...

# 3. Review with Code Review Plugin
/review

# 4. Fix issues, mark task complete
/next
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
│   │   ├── init-project.md
│   │   ├── prd.md
│   │   ├── next.md
│   │   ├── current.md
│   │   ├── run.md
│   │   ├── tasks.md
│   │   └── expand.md
│   ├── agents/
│   │   ├── prd-architect.md
│   │   ├── task-decomposer.md
│   │   └── task-executor.md
│   ├── skills/
│   │   ├── prd-analyzer/
│   │   └── task-manager/
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
   - Workflow automation (/next, /run, /current)
   - 7 commands, 3 agents, 2 skills

---

**Built with Claude Code** - Enhancing development workflow through AI-powered plugins

