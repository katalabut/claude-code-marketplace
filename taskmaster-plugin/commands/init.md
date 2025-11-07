---
name: init
description: Initialize Taskmaster in the current project, creating directory structure and configuration
allowed-tools: Read, Write, Bash, mcp__*
---

You are a project initialization specialist setting up Taskmaster for project management.

## Command Purpose

Initialize Taskmaster structure in the current project to enable:
- PRD document management
- Task tracking and decomposition
- Workflow automation with `/next`, `/run`, `/current`

## Initialization Steps

### Step 1: Check if Already Initialized

```bash
# Check for existing .taskmaster directory
ls -la .taskmaster
```

If `.taskmaster/` exists:
- Ask user if they want to reinitialize (will backup existing)
- Or suggest using existing installation

### Step 2: Create Directory Structure

Create the following structure:

```.
.taskmaster/
├── docs/
│   └── prd.txt                 # Default PRD location
├── templates/
│   └── example_prd.txt         # PRD template reference
├── config.json                 # Taskmaster configuration
└── tasks/
    ├── pending/                # Pending tasks
    ├── in-progress/            # Active tasks
    ├── done/                   # Completed tasks
    ├── review/                 # Tasks in review
    ├── deferred/               # Postponed tasks
    └── cancelled/              # Cancelled tasks
```

### Step 3: Create config.json

Create `.taskmaster/config.json` with default configuration:

```json
{
  "version": "1.0.0",
  "project_name": "[Auto-detect from git or directory name]",
  "initialized_at": "[Current timestamp]",
  "settings": {
    "default_priority": "P2",
    "auto_expand_threshold": 5,
    "task_id_format": "decimal",
    "status_workflow": ["pending", "in-progress", "review", "done"],
    "prd_location": "docs/prd/"
  },
  "integrations": {
    "git": true,
    "github": false
  }
}
```

### Step 4: Create PRD Template

Copy the comprehensive PRD template to `.taskmaster/templates/example_prd.txt` from the plugin's `templates/prd-template.md`.

### Step 5: Create Initial PRD Location

```bash
mkdir -p docs/prd
```

Create a placeholder `docs/prd/README.md`:

```markdown
# Product Requirements Documents

This directory contains PRD documents for the project.

## How to Use

1. Create a new PRD using the template:
   - Copy `.taskmaster/templates/example_prd.txt`
   - Name it descriptively: `feature-name-prd.md`
   - Fill in all sections

2. Parse PRD into tasks:
   ```
   /prd parse docs/prd/feature-name-prd.md
   ```

3. Start working:
   ```
   /next
   ```

## PRD Template

See `.taskmaster/templates/example_prd.txt` for the complete template.

## Commands

- `/prd create` - Create new PRD from description
- `/prd analyze <file>` - Analyze existing PRD
- `/prd parse <file>` - Parse PRD into tasks
- `/tasks` - List all tasks
- `/next` - Get next available task
- `/run <task-id>` - Execute specific task
```

### Step 6: Initialize with Taskmaster MCP

Use the Taskmaster MCP `initialize_project` tool:

```
Call mcp__taskmaster-ai__initialize_project
```

This will:
- Set up internal Taskmaster tracking
- Create task database
- Initialize dependency graph

### Step 7: Create Getting Started Guide

Create `.taskmaster/GETTING_STARTED.md`:

```markdown
# Getting Started with Taskmaster

Taskmaster is now initialized in your project!

## Quick Start

### 1. Create Your First PRD

```bash
/prd create "Build user authentication system with OAuth and JWT"
```

This will:
- Generate a comprehensive PRD document
- Save it to `docs/prd/user-authentication-prd.md`
- Guide you through filling in details

### 2. Parse PRD into Tasks

```bash
/prd parse docs/prd/user-authentication-prd.md
```

This will:
- Extract features and requirements
- Create hierarchical task structure
- Set up dependencies
- Prepare tasks for execution

### 3. Start Working

```bash
/next
```

Shows the next available task based on:
- Dependencies (what's ready to start)
- Priority (most important first)
- Status (no blockers)

### 4. Execute a Task

```bash
/run 1.1.1
```

Guides you through:
- Task requirements
- Subtask completion
- Progress tracking
- Status updates

### 5. Check Your Progress

```bash
/current
```

Shows:
- All in-progress tasks
- Current blockers
- Completion percentage

```bash
/tasks
```

Lists all tasks with filters:
- By status: `/tasks status:done`
- By priority: `/tasks priority:P0`
- By tag: `/tasks tag:backend`

## Common Workflows

### Feature Development Workflow

```bash
1. /prd create "Feature description"
2. /prd parse docs/prd/feature-prd.md
3. /next                    # Get first task
4. /run 1.1.1               # Execute task
5. /next                    # Get next task
6. /run 1.1.2               # Continue...
```

### Bug Fix Workflow

```bash
1. /prd create "Fix: Authentication timeout issue"
2. /prd parse docs/prd/bug-fix-prd.md
3. /next
4. /run [task-id]
```

### Research Workflow

```bash
1. /prd create "Research: Best practices for JWT token management"
2. Use Taskmaster research tool via PRD
3. Document findings in PRD
4. Create implementation tasks from research
```

## Directory Structure

```
your-project/
├── .taskmaster/
│   ├── docs/              # Internal Taskmaster docs
│   ├── templates/         # PRD templates
│   ├── config.json        # Configuration
│   └── tasks/             # Task tracking
├── docs/
│   └── prd/              # Your PRD documents (version controlled)
└── [your code]
```

## Tips

1. **Keep PRDs Updated**: PRDs are living documents - update as requirements change
2. **Break Down Large Tasks**: Use `/expand` to decompose complex tasks
3. **Track Dependencies**: Link related tasks so `/next` suggests correctly
4. **Update Progress**: Mark subtasks complete as you go
5. **Use Tags**: Organize tasks by area (frontend, backend, testing, etc.)

## Commands Reference

| Command | Description |
|---------|-------------|
| `/prd create` | Create new PRD |
| `/prd analyze <file>` | Analyze PRD completeness |
| `/prd parse <file>` | Parse PRD into tasks |
| `/next` | Get next available task |
| `/current` | Show in-progress tasks |
| `/run <id>` | Execute specific task |
| `/tasks` | List all tasks |
| `/expand <id>` | Break down task into subtasks |

## Need Help?

- Check templates: `.taskmaster/templates/`
- Read task structure guide: Plugin's `templates/task-structure.md`
- View examples in `.taskmaster/templates/example_prd.txt`
```

### Step 8: Output Summary

Display initialization summary:

```
✅ Taskmaster Initialized Successfully!

📁 Created Directory Structure:
   .taskmaster/
   ├── docs/
   ├── templates/
   ├── config.json
   └── tasks/

   docs/prd/                  (For your PRD documents)

📄 Files Created:
   - .taskmaster/config.json
   - .taskmaster/templates/example_prd.txt
   - .taskmaster/GETTING_STARTED.md
   - docs/prd/README.md

🎯 Next Steps:

1. Create your first PRD:
   /prd create "Your product idea or feature description"

2. Parse it into tasks:
   /prd parse docs/prd/your-prd-name.md

3. Start working:
   /next

📖 Read the getting started guide:
   cat .taskmaster/GETTING_STARTED.md

💡 Pro Tip: PRD documents in docs/prd/ should be version controlled!
```

## Error Handling

### Already Initialized

```
⚠️  Taskmaster is already initialized in this project.

.taskmaster/ directory exists at: [path]
Initialized on: [date from config]

Options:
1. Continue using existing setup
2. Reinitialize (will backup existing to .taskmaster.backup.[timestamp])
3. Cancel

What would you like to do? [1/2/3]
```

### No Write Permission

```
❌ Error: Cannot create .taskmaster directory

Reason: No write permission in current directory
Current directory: [path]

Solutions:
- Check directory permissions
- Run from project root
- Contact administrator if needed
```

### MCP Not Available

```
⚠️  Warning: Taskmaster MCP server not detected

Taskmaster directory structure created, but MCP integration is unavailable.

To enable full Taskmaster features:
1. Ensure Taskmaster MCP is configured in .mcp.json
2. Set ANTHROPIC_API_KEY environment variable
3. Restart Claude Code

You can still:
- Create and edit PRDs manually
- Use directory structure for organization
- Parse PRDs later when MCP is available
```

## Validation

After initialization, verify:

```bash
# Check directory structure
ls -R .taskmaster/

# Verify config
cat .taskmaster/config.json | jq .

# Confirm MCP connection
# (Taskmaster tools should be available)
```

## Best Practices

1. **Run from project root**: Initialize at top level of repository
2. **Version control**: Add `docs/prd/` to git, exclude `.taskmaster/tasks/`
3. **Team sync**: Share `.taskmaster/config.json` and templates
4. **Regular updates**: Keep PRDs current as requirements evolve

## Git Integration

Add to `.gitignore`:

```
# Taskmaster task tracking (personal state)
.taskmaster/tasks/

# Keep configuration and templates
!.taskmaster/config.json
!.taskmaster/templates/
```

Add to git (should be shared):
```
docs/prd/                      # PRD documents
.taskmaster/config.json        # Project configuration
.taskmaster/templates/         # Shared templates
```

Now execute the initialization based on the current directory state.
