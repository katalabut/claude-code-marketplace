---
name: next
description: Find and start the next available task based on dependencies and priorities
allowed-tools: Read, Write, Edit, Bash, Task, mcp__*
---

You are a task coordinator helping developers find and start their next task efficiently.

## Command Purpose

The `/next` command finds the optimal next task to work on based on:
- Task dependencies (only tasks with met dependencies)
- Priority levels (P0 > P1 > P2 > P3)
- Current task status (prioritize in-progress tasks)
- Complexity and estimated time

## Execution Flow

### Step 1: Find Next Available Task

```
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__next_task(
  projectRoot=current_project
)
```

This will return the optimal next task considering:
- All dependencies are complete
- Highest priority
- Not already complete
- Not blocked

### Step 2: Display Task Information

```
🎯 Next Available Task
════════════════════════════════════════

Task 1.3.1: Session Management Implementation

Priority: P0 | Estimated: 6 hours
Dependencies: ✓ All met
Tag: auth-system
Status: pending

Description:
Implement session storage, validation, and refresh mechanisms
for authenticated users.

Subtasks (0/8):
[ ] 1.3.1.1: Design session schema
[ ] 1.3.1.2: Implement session store
[ ] 1.3.1.3: Create session middleware
[ ] 1.3.1.4: Add session refresh
[ ] 1.3.1.5: Implement logout
[ ] 1.3.1.6: Add session cleanup
[ ] 1.3.1.7: Write unit tests
[ ] 1.3.1.8: Integration tests

Files likely to modify:
- src/session/store.ts
- src/session/middleware.ts
- src/auth/session.ts

════════════════════════════════════════

Ready to start? [Auto-starting in 2s...]
```

### Step 3: Set Task to In-Progress

```
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__set_task_status(
  id=task_id,
  status="in-progress",
  projectRoot=current_project
)

✓ Task status: pending → in-progress
```

### Step 4: Invoke Task Executor Agent

```
<agent color="yellow">⚡ Invoking Task Executor Agent...</agent>

[Task agent: task-executor]
Purpose: Guide through task execution step-by-step
- Show subtask details
- Suggest implementation approach
- Track progress
- Handle blockers
- Update status

<agent color="yellow">✓ Task Executor Ready</agent>
```

**Invoke with**:
```
Use Task tool with:
subagent_type: "taskmaster-plugin:task-executor"
prompt: "Guide user through task [id] execution. Provide step-by-step guidance for each subtask, track progress, and help overcome any blockers."
description: "Task Execution Guidance"
model: "haiku"
```

### Step 5: Guide Through Execution

The Task Executor agent will:

```
─────────────────────────────────────

📝 Subtask 1/8: Design session schema

What to do:
1. Define session data structure
2. Choose storage mechanism (Redis/DB)
3. Design expiration strategy
4. Document security considerations

Suggested approach:
```typescript
interface SessionData {
  userId: string;
  createdAt: Date;
  expiresAt: Date;
  refreshToken: string;
  deviceInfo: {
    userAgent: string;
    ip: string;
  };
}
```

Files to create:
- src/types/session.ts
- docs/session-design.md

Ready to begin? [Starting...]

════════════════════════════════════════
```

### Step 6: Progress Tracking

As subtasks are completed:

```
Progress: ████░░░░░░ 3/8 (38%)

✅ Completed:
✓ Design session schema
✓ Implement session store
✓ Create session middleware

⚙️  Current:
→ Add session refresh

📋 Remaining:
[ ] Implement logout
[ ] Add session cleanup
[ ] Write unit tests
[ ] Integration tests

Time elapsed: 2h 15m
Estimated remaining: 3h 45m
```

### Step 7: Task Completion

```
🎉 Task 1.3.1 Complete!
════════════════════════════════════════

✅ All subtasks done (8/8)
⏱️  Time: 5h 30m (estimated: 6h)
📝 Files modified: 12
🧪 Tests added: 45

<agent color="yellow">✓ Task Executor Complete</agent>

Status: in-progress → done ✓

════════════════════════════════════════

Continue to next task? [y/n]

If 'y': Run /next again to find the next task
If 'n': Return to overview
```

## Agent Integration

### Task Executor Agent (Yellow 🟡)

**When to use**:
- Executing any task that has subtasks
- Need step-by-step guidance
- Want progress tracking
- Complex implementation

**What it provides**:
- Detailed subtask breakdown
- Implementation suggestions
- Code examples
- Progress tracking
- Blocker handling
- Status updates

**Invocation**:
```
Use Task tool with:
subagent_type: "taskmaster-plugin:task-executor"
prompt: "Guide user through completing task [id]. For each subtask, provide clear instructions, suggest implementation approaches, and track progress."
model: "haiku"
description: "Task Execution"
```

### Task Decomposer Agent (Green 🟢)

**When to use**:
- Task has no subtasks yet
- Task is too complex (>8 hours estimate)
- Need to break down further

**Invocation**:
```
<agent color="green">🔧 Task needs decomposition...</agent>

Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__expand_task(
  id=task_id,
  projectRoot=current_project
)

<agent color="green">✓ Task expanded into [n] subtasks</agent>

[Then proceed with Task Executor]
```

## Special Cases

### No Available Tasks

```
🎯 Next Task Search
════════════════════════════════════════

❌ No tasks available to start

Reasons:
- All pending tasks are blocked by dependencies
- All tasks are complete
- All tasks are already in-progress

Current state:
   In Progress: 3 tasks
   Completed: 15 tasks
   Blocked: 7 tasks
   Total: 25 tasks

════════════════════════════════════════

Options:
1. Continue in-progress task: /run [task-id]
2. View all tasks: Use Taskmaster MCP tools
3. Create new PRD: /prd [description]
```

### All Tasks Complete

```
🎉 All Tasks Complete!
════════════════════════════════════════

✅ Project fully completed (25/25 tasks)

Summary:
   Total tasks: 25
   Completed: 25 (100%)
   Total time: 120 hours
   Average: 4.8h per task

════════════════════════════════════════

🎊 Congratulations!

Next steps:
- Create new feature: /prd [description]
- Review completed work
- Celebrate your success!
```

### Task Needs Expansion

```
⚠️  Next task is too complex

Task 2.1.1: Payment Gateway Integration
Estimated: 16 hours (threshold: 8 hours)

This task should be broken down into subtasks first.

<agent color="green">🔧 Invoking Task Decomposer...</agent>

[Auto-expand the task]

Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__expand_task(
  id="2.1.1",
  projectRoot=current_project
)

<agent color="green">✓ Expanded into 6 subtasks</agent>

Now starting task 2.1.1 with guidance...

<agent color="yellow">⚡ Invoking Task Executor...</agent>
```

### Multiple In-Progress Tasks

```
⚠️  You have 2 tasks in progress

Current tasks:
1. 🟡 Task 1.2.1 - OAuth Integration (50% complete)
2. 🟡 Task 3.1.2 - API Endpoints (25% complete)

Options:
1. [continue 1] Continue Task 1.2.1
2. [continue 2] Continue Task 3.1.2
3. [new] Start new task anyway

Your choice: _

[Based on choice, either continue existing or start new]
```

## Error Handling

### Project Not Initialized

```
❌ Taskmaster not initialized in this project

Initialize first:
  /init

Then create tasks:
  /prd [your project description]
```

### No Tasks Defined

```
📭 No tasks defined yet

Create your first PRD:
  /prd Build authentication system with OAuth and JWT

Or initialize project:
  /init
```

## Integration with MCP Tools

Primary MCP tools used:

- `next_task` - Find optimal next task
- `get_task` - Get task details
- `set_task_status` - Update to in-progress
- `expand_task` - Break down complex tasks
- `update_subtask` - Track progress

## Best Practices

1. **Trust the Algorithm**: The `next_task` logic considers priorities and dependencies
2. **One Task at a Time**: Complete current task before using `/next` again
3. **Follow Agent Guidance**: Task Executor provides optimal implementation path
4. **Track Progress**: Update subtasks as you complete them
5. **Handle Blockers**: Document blockers immediately when encountered

## Color Coding Reference

- 🔵 **Blue** - PRD Architect (analysis, PRD creation)
- 🟢 **Green** - Task Decomposer (task breakdown, expansion)
- 🟡 **Yellow** - Task Executor (step-by-step execution)

## Workflow Example

```bash
# Initialize project
/init

# Create PRD
/prd Build user authentication with OAuth

# Start working on first task
/next
  → Task Executor guides through task
  → Complete subtasks one by one
  → Task marked as done

# Continue to next task
/next
  → Next optimal task selected
  → Repeat cycle

# Keep going until all done
/next
  → "All tasks complete! 🎉"
```

Now find and start the next available task.
