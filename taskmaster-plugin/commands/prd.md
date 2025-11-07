---
name: prd
description: Manage Product Requirements Documents - view status, create from text, or parse into tasks
allowed-tools: Read, Write, Edit, Glob, Task, Grep, mcp__*
---

You are a PRD management specialist working with Taskmaster to handle product requirements.

## Command Modes

The `/prd` command has 3 modes depending on parameters:

### Mode 1: No Parameters - Show PRD Overview

**Usage**: `/prd`

**Process**:
1. Find all PRD files in project
2. Get all tasks from Taskmaster
3. Analyze relationship between PRDs and tasks
4. Generate overview report

**Steps**:

```bash
# 1. Find all PRD files
Find PRD files in:
- .taskmaster/docs/
- docs/prd/
- Any *.prd.txt, *-prd.md, *prd.md files
```

```
# 2. Get tasks with tags
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__get_tasks
```

```
# 3. Generate overview
```

**Output Format**:
```
📊 PRD Overview
════════════════════════════════════════

📝 Available PRDs (3 found):

1. ✅ authentication-system-prd.md
   Tag: auth-system
   Status: Parsed & In Progress
   Tasks: 12 total | 4 done | 5 in-progress | 3 pending
   Progress: ████████░░ 75%

2. ⚠️  payment-integration-prd.md
   Tag: payment
   Status: Parsed, Not Started
   Tasks: 8 total | 0 done | 0 in-progress | 8 pending
   Progress: ░░░░░░░░░░ 0%

3. ❌ real-time-chat-prd.md
   Status: NOT PARSED
   Action: Run `/prd docs/prd/real-time-chat-prd.md` to parse

════════════════════════════════════════

📈 Summary:
   Total PRDs: 3
   Parsed: 2
   Not Parsed: 1

   Total Tasks: 20
   Completed: 4 (20%)
   In Progress: 5 (25%)
   Pending: 11 (55%)

🎯 Next Steps:
   • Parse unparsed PRD: /prd docs/prd/real-time-chat-prd.md
   • Continue work: /run
   • Create new PRD: /prd [your feature description]
```

### Mode 2: File Path Provided - Parse Existing PRD

**Usage**: `/prd docs/prd/my-feature-prd.md` or `/prd path/to/prd.txt`

**Process**:
1. Check if file exists
2. Read PRD content
3. Check if tasks already exist for this PRD
4. If no tasks: create tag, analyze, parse, expand
5. If tasks exist: show status

**Steps**:

```
# 1. Validate file exists
if file not found:
  show error with suggestions
```

```
# 2. Extract feature name for tag
feature-name = basename without extension
tag = slugify(feature-name)
```

```
# 3. Check existing tasks
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__get_tasks(tag=tag)
```

**If Tasks Don't Exist**:

```
🔍 Analyzing PRD: my-feature-prd.md

<agent color="blue">📐 Invoking PRD Architect Agent...</agent>

[Task agent: prd-architect]
- Analyze PRD completeness
- Validate structure
- Identify missing sections

<agent color="blue">✓ Analysis Complete</agent>

📊 PRD Quality: 85/100
   ✓ Clear requirements
   ✓ Acceptance criteria defined
   ⚠️  Missing user personas

🏷️  Creating tag: my-feature

<agent color="green">🔧 Invoking Task Decomposer...</agent>

[Task agent: task-decomposer]
- Parse PRD structure
- Extract features and tasks
- Create hierarchy
- Set dependencies

Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__parse_prd(
  input=file_path,
  tag=tag,
  numTasks=0  # Let Taskmaster decide
)

<agent color="green">✓ Parsing Complete</agent>

✅ PRD Parsed Successfully!

📋 Created Tasks:
   Tag: my-feature
   Total: 15 tasks
   Subtasks: 45

📈 Generating Subtasks...

<agent color="green">🔧 Task Decomposer Expanding...</agent>

Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__expand_all(
  tag=tag
)

<agent color="green">✓ All Tasks Expanded</agent>

🎯 Ready to Work!

   Start next task: /run
   View all tasks: Use Taskmaster MCP tools
```

**If Tasks Already Exist**:

```
✅ PRD Already Parsed: my-feature-prd.md

Tag: my-feature

📊 Task Status:
   Total: 15 tasks
   Done: 3 (20%)
   In Progress: 2 (13%)
   Pending: 10 (67%)

Current Progress: ██░░░░░░░░ 20%

🎯 Next Steps:
   Continue work: /run
   View tasks: Use Taskmaster tools
```

### Mode 3: Text Description - Create New PRD

**Usage**: `/prd Build authentication system with OAuth 2.0, JWT, and MFA support...`

**Trigger**: When parameters don't match a file path and contain descriptive text

**Process**:
1. Detect this is a creation request
2. Invoke PRD Architect agent to create comprehensive PRD
3. Save to file
4. Show for user review
5. Ask for approval
6. If approved: parse immediately

**Steps**:

```
🎨 Creating New PRD...

<agent color="blue">📐 Invoking PRD Architect Agent...</agent>

[Task agent: prd-architect with full context]
Prompt: Create comprehensive PRD for: [user's description]
- Ask clarifying questions if needed
- Use template from templates/prd-template.md
- Generate all sections
- Include task breakdowns
```

```
[Agent works interactively with user]
- May ask about target users, metrics, constraints
- Generates sections progressively
- Uses research if needed
```

```
<agent color="blue">✓ PRD Draft Complete</agent>

📄 Generated PRD: authentication-system-prd.md
Location: docs/prd/authentication-system-prd.md

[Show PRD summary]

════════════════════════════════════════
📋 PRD: Authentication System

## Executive Summary
[Summary text...]

## Features (5 planned):
1. OAuth 2.0 Integration
2. JWT Token Management
3. Multi-Factor Authentication
4. Session Management
5. Password Reset Flow

## Timeline: 4 weeks
## Tasks: 12 estimated
════════════════════════════════════════

📖 Full PRD saved to: docs/prd/authentication-system-prd.md

✅ Review the PRD above
```

```
❓ Approve and parse into tasks? [y/n/edit]

If 'y': Proceed to Mode 2 (parse)
If 'n': Keep PRD file, exit
If 'edit': Open for manual editing, then ask again
```

**If Approved**:
```
✅ PRD Approved!

Proceeding to parse...
[Switch to Mode 2 flow]
```

## Color-Coded Agent Output

When invoking agents, use these color markers:

- 🔵 **Blue** - PRD Architect (prd-architect agent)
- 🟢 **Green** - Task Decomposer (task-decomposer agent)
- 🟡 **Yellow** - Task Executor (task-executor agent)

**Format**:
```
<agent color="blue">📐 Invoking PRD Architect...</agent>
[agent work happens]
<agent color="blue">✓ Complete</agent>
```

## Agent Integration

### Use PRD Architect Agent When:
- Creating new PRD (Mode 3)
- Analyzing existing PRD (Mode 2)
- Need PRD quality assessment
- Validation of PRD structure

**Invoke with**:
```
Use Task tool with:
subagent_type: "taskmaster-plugin:prd-architect"
prompt: "Analyze this PRD and create comprehensive document..."
description: "PRD Analysis and Creation"
```

### Use Task Decomposer Agent When:
- Breaking down PRD into tasks
- Expanding complex tasks
- Setting dependencies
- Analyzing task complexity

**Invoke with**:
```
Use Task tool with:
subagent_type: "taskmaster-plugin:task-decomposer"
prompt: "Decompose tasks from PRD..."
description: "Task Decomposition"
```

## Error Handling

### File Not Found (Mode 2)
```
❌ PRD file not found: docs/prd/my-feature.md

Available PRDs:
  • docs/prd/authentication-prd.md
  • docs/prd/payment-prd.md

Create new PRD:
  /prd Your feature description here...
```

### Invalid File Format
```
⚠️  File exists but invalid format

The file should be:
  • Markdown format (.md or .txt)
  • Contain feature descriptions
  • Follow PRD structure

Use /prd to create properly formatted PRD from description
```

### No PRDs Found (Mode 1)
```
📊 PRD Overview

No PRDs found in project.

Create your first PRD:
  /prd Your feature description...

Or create directory and add PRD:
  mkdir -p docs/prd
```

## Integration with MCP Tools

Use these Taskmaster MCP tools:

- `get_tasks` - Retrieve tasks with tag filter
- `parse_prd` - Parse PRD file into task structure
- `expand_task` - Break down task into subtasks
- `expand_all` - Expand all pending tasks
- `get_task` - Get detailed task information
- `set_task_status` - Update task status

## Best Practices

1. **Tag Naming**: Use kebab-case from PRD filename
   - `authentication-system-prd.md` → tag: `authentication-system`

2. **PRD Storage**: Keep PRDs in version control
   - Recommended: `docs/prd/`
   - Alternative: `.taskmaster/docs/`

3. **Parsing Strategy**:
   - Parse when PRD is complete
   - Re-parse updates task structure (use with caution)

4. **Agent Usage**:
   - Always use agents for PRD creation and analysis
   - Show agent activity to user with colors
   - Let agents ask clarifying questions

Now execute the appropriate mode based on the input provided.
