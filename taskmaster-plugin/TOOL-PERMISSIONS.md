# Tool Permissions Guide

This document explains MCP tool access configuration for the Taskmaster plugin.

## MCP Taskmaster Tools

### Tool Loading Modes

Configure via `TASK_MASTER_TOOLS` environment variable in `.mcp.json`:

| Mode | Tools | Context | Use Case |
|------|-------|---------|----------|
| `core` | 7 tools | ~5K tokens | Essential workflow only |
| `standard` | 15 tools | ~10K tokens | **Recommended** - balanced |
| `all` | 32 tools | ~21K tokens | Complete feature set |

**Default**: `standard` mode (configured in plugin)

### Core Tools (7)

Minimum essential tools:

1. **get_tasks** - List and filter tasks
2. **next_task** - Find next available task
3. **get_task** - Get detailed task info
4. **set_task_status** - Update task status
5. **update_subtask** - Add progress notes
6. **parse_prd** - Parse PRD into tasks
7. **expand_task** - Break down complex tasks

### Standard Tools (15)

Core + additional workflow tools:

8. **add_dependency** - Set task dependencies
9. **remove_dependency** - Remove dependencies
10. **validate_dependencies** - Check circular deps
11. **initialize_project** - Setup project structure
12. **research** - AI-powered research
13. **add_task** - Create new tasks
14. **add_subtask** - Add subtasks
15. **move_task** - Reorganize hierarchy

### All Tools (32)

Standard + advanced features (see Taskmaster docs for complete list)

## Component Permissions

### Commands

All commands use:
```yaml
allowed-tools: Read, Write, Edit, Bash
```

Plus MCP tools as needed:
- `/init-project`: Uses `initialize_project`
- `/prd`: Uses `parse_prd`, `research`
- `/next`: Uses `next_task`, `get_task`
- `/current`: Uses `get_tasks`
- `/run`: Uses `get_task`, `update_subtask`, `set_task_status`
- `/tasks`: Uses `get_tasks`
- `/expand`: Uses `expand_task`, `add_dependency`

### Agents

**prd-architect**:
```yaml
tools: Read, Write, Edit
```
MCP: `parse_prd`, `research`

**task-decomposer**:
```yaml
tools: Read
```
MCP: `expand_task`, `add_dependency`, `validate_dependencies`

**task-executor**:
```yaml
tools: Read, Write, Edit, Bash
```
MCP: `get_task`, `update_subtask`, `set_task_status`

### Skills

**prd-analyzer**:
```yaml
allowed-tools: Read, Write
```
MCP: `parse_prd`

**task-manager**:
```yaml
allowed-tools: Read
```
MCP: `get_tasks`, `next_task`, `set_task_status`

## Environment Variables

### Required

```bash
export ANTHROPIC_API_KEY="sk-ant-your-key"
```

Taskmaster uses this for:
- Task AI analysis
- PRD parsing
- Task expansion
- Research capabilities

### Optional

```bash
export PERPLEXITY_API_KEY="pplx-your-key"
```

Enhances research tool with fresh data beyond Claude's knowledge cutoff.

### Custom Tool Mode

```bash
export TASK_MASTER_TOOLS="core"     # Minimal
export TASK_MASTER_TOOLS="standard" # Default
export TASK_MASTER_TOOLS="all"      # Maximum
```

## MCP Configuration

The `.mcp.json` file at plugin root:

```json
{
  "taskmaster-ai": {
    "command": "npx",
    "args": ["-y", "task-master-ai"],
    "env": {
      "ANTHROPIC_API_KEY": "${ANTHROPIC_API_KEY}",
      "TASK_MASTER_TOOLS": "standard"
    }
  }
}
```

### Key Points

- `npx -y task-master-ai`: Auto-installs latest Taskmaster
- Environment variables: Use `${VAR}` syntax for substitution
- Tool mode: Set via `TASK_MASTER_TOOLS` env var

## Troubleshooting

### MCP Server Not Starting

```bash
# Check configuration
cat .mcp.json

# Verify API key is set
echo $ANTHROPIC_API_KEY

# Check MCP status in Claude Code
/mcp
```

### Tool Not Available

Check tool mode supports the tool you need:
- Core: 7 essential tools only
- Standard: 15 common tools
- All: 32 complete toolset

To use more tools, change mode in `.mcp.json`:
```json
"TASK_MASTER_TOOLS": "all"
```

Then restart Claude Code.

### Permission Denied

MCP tools are automatically available to:
- All commands (via `allowed-tools`)
- All agents (via `tools`)
- All skills (via `allowed-tools`)

No additional permissions needed beyond MCP server running.

## Security Best Practices

1. **Never commit API keys**: Use environment variables only
2. **Limit tool access**: Use `core` or `standard` mode unless needed
3. **Review tool calls**: Monitor what tools are invoked
4. **Restrict file access**: Commands have limited tool access by design
5. **Validate MCP config**: Check `.mcp.json` syntax before committing

## Tool Usage by Feature

| Feature | Required MCP Tools |
|---------|-------------------|
| Initialize project | `initialize_project` |
| Create PRD | `research` (optional) |
| Parse PRD | `parse_prd` |
| Get next task | `next_task`, `get_task` |
| View tasks | `get_tasks` |
| Execute task | `get_task`, `update_subtask`, `set_task_status` |
| Expand task | `expand_task`, `add_dependency` |
| Track progress | `get_tasks`, `get_task` |

## Changing Configuration

### Switch Tool Mode

Edit `.mcp.json`:
```json
{
  "taskmaster-ai": {
    "env": {
      "TASK_MASTER_TOOLS": "all"  # Changed from "standard"
    }
  }
}
```

Restart Claude Code for changes to take effect.

### Add Additional MCP Servers

The `.mcp.json` supports multiple servers:
```json
{
  "taskmaster-ai": { ... },
  "github-tools": { ... },
  "another-server": { ... }
}
```

Each plugin can configure its own MCP servers independently.

## References

- Taskmaster MCP: https://github.com/eyaltoledano/claude-task-master
- MCP Protocol: https://modelcontextprotocol.io/
- Claude Code MCP Docs: https://code.claude.com/docs/en/mcp
- Taskmaster Tools Reference: https://deepwiki.com/eyaltoledano/claude-task-master/5.1-mcp-tools

## Version

1.0.0
