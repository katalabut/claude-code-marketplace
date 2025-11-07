# Taskmaster Plugin for Claude Code

Complete project management with Taskmaster MCP: PRD creation, task decomposition, and workflow automation.

## Features

- 📝 **PRD Management**: Create and analyze Product Requirements Documents
- 🔄 **Task Decomposition**: Break down complex tasks intelligently  
- 🎯 **Workflow Automation**: `/next`, `/run`, `/current` commands
- 📊 **Progress Tracking**: Real-time status and completion monitoring
- 🤖 **AI-Powered**: Taskmaster MCP integration with Claude

## Quick Start

### 1. Prerequisites

```bash
# Install Taskmaster MCP (automatic via .mcp.json)
# Set API key
export ANTHROPIC_API_KEY="your-key-here"
```

### 2. Initialize Project

```bash
/init-project
```

Creates `.taskmaster/` structure and configuration.

### 3. Create Your First PRD

```bash
/prd create "Build user authentication with OAuth and JWT"
```

### 4. Parse into Tasks

```bash
/prd parse docs/prd/user-authentication-prd.md
```

### 5. Start Working

```bash
/next              # Get next task
/run 1.1.1         # Execute task
/current           # Check progress
```

## Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `/init-project` | Initialize Taskmaster | `/init-project` |
| `/prd create` | Create new PRD | `/prd create "Description"` |
| `/prd analyze` | Analyze PRD | `/prd analyze docs/prd/file.md` |
| `/prd parse` | Parse into tasks | `/prd parse docs/prd/file.md` |
| `/next` | Get next task | `/next` |
| `/current` | Show in-progress | `/current` |
| `/run` | Execute task | `/run 1.2.1` |
| `/tasks` | List all tasks | `/tasks` or `/tasks status:pending` |
| `/expand` | Break down task | `/expand 1.2.1` |

## Agents

### prd-architect
Creates comprehensive PRDs following best practices.
- Auto-invokes when creating PRDs
- Uses industry-standard templates
- Includes task breakdowns

### task-decomposer  
Breaks down complex tasks into subtasks.
- Analyzes task complexity
- Sets proper dependencies
- Estimates effort

### task-executor
Guides through task execution step-by-step.
- Tracks progress
- Updates status
- Identifies blockers

## Skills

### prd-analyzer
Analyzes PRD documents automatically.
- Validates completeness
- Extracts requirements
- Suggests improvements

### task-manager
Manages workflow and suggests next actions.
- Tracks all task statuses
- Identifies blockers
- Optimizes workflow

## Workflow Examples

### Feature Development

```bash
# 1. Create PRD
/prd create "Real-time chat with WebSockets"

# 2. Parse into tasks
/prd parse docs/prd/real-time-chat-prd.md

# 3. Review tasks
/tasks

# 4. Start working
/next
/run 1.1.1

# 5. Continue workflow
/next
/run 1.1.2

# 6. Check progress
/current
```

### Bug Fix

```bash
# 1. Document in PRD
/prd create "Fix: Authentication timeout issue"

# 2. Parse
/prd parse docs/prd/bug-fix-prd.md

# 3. Execute
/next
/run [task-id]
```

## Directory Structure

```
your-project/
├── .taskmaster/              # Taskmaster data
│   ├── config.json          # Configuration
│   ├── templates/           # PRD templates
│   └── tasks/              # Task tracking
├── docs/
│   └── prd/                # Your PRD documents
│       ├── feature-1.md
│       └── feature-2.md
└── [your code]
```

## Configuration

### Environment Variables

```bash
# Required
export ANTHROPIC_API_KEY="sk-ant-..."

# Optional (for enhanced research)
export PERPLEXITY_API_KEY="pplx-..."
```

### MCP Configuration (`.mcp.json`)

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

Tool modes:
- `core` (7 tools, ~5K tokens) - Essential only
- `standard` (15 tools, ~10K tokens) - **Default, recommended**
- `all` (32 tools, ~21K tokens) - Complete feature set

## MCP Tools Used

Core tools (standard mode):
- `get_tasks` - List/filter tasks
- `next_task` - Find next available
- `get_task` - Get details
- `set_task_status` - Update status
- `update_subtask` - Track progress
- `parse_prd` - Parse PRD files
- `expand_task` - Break down tasks
- `add_dependency` - Manage dependencies
- `initialize_project` - Setup structure
- `research` - AI research

## Task Structure

Uses hierarchical decimal notation:

```
1. Theme: User Authentication
  1.1. Epic: OAuth 2.0
    1.1.1. Feature: Google OAuth
      1.1.1.1. Task: Setup config
        1.1.1.1.1. Subtask: Create project
        1.1.1.1.2. Subtask: Get credentials
      1.1.1.2. Task: Implement flow
    1.1.2. Feature: GitHub OAuth
  1.2. Epic: Multi-Factor Auth
```

## PRD Template Sections

1. Document Information
2. Executive Summary
3. Problem Statement
4. Goals & Success Metrics
5. Target Audience & Personas
6. Features & Requirements
7. Technical Architecture
8. Timeline & Milestones
9. Testing Strategy
10. Go-to-Market Strategy

See `templates/prd-template.md` for complete template.

## Best Practices

1. **Initialize first**: Run `/init-project` before starting
2. **PRDs in version control**: Keep `docs/prd/` in git
3. **Update regularly**: PRDs are living documents
4. **Break down large tasks**: Use `/expand` for complex work
5. **Track progress**: Update subtasks as you complete them
6. **Use `/next`**: Let algorithm suggest optimal task order

## Troubleshooting

### MCP Not Connected

```bash
# Check MCP status
/mcp

# Verify API key
echo $ANTHROPIC_API_KEY

# Restart Claude Code
```

### No Tasks Available

```bash
# Check if project initialized
ls .taskmaster/

# Check if PRD parsed
/tasks

# Create PRD if needed
/prd create "Your feature"
/prd parse docs/prd/your-feature.md
```

### Task Dependencies Not Met

```bash
# Check dependency status
/tasks [task-id]

# Complete prerequisite tasks first
/next  # Shows only ready tasks
```

## Git Integration

### `.gitignore`

```
# Task state (personal)
.taskmaster/tasks/

# Keep configuration
!.taskmaster/config.json
!.taskmaster/templates/
```

### Version Control

Commit these:
- `docs/prd/` - PRD documents
- `.taskmaster/config.json` - Project config  
- `.taskmaster/templates/` - Custom templates

Don't commit:
- `.taskmaster/tasks/` - Personal task state

## Support

- Documentation: `templates/` directory
- Task structure guide: `templates/task-structure.md`
- PRD template: `templates/prd-template.md`
- Tool permissions: `TOOL-PERMISSIONS.md`

## License

MIT License

## Version

1.0.0
