---
name: run
description: Start working on tasks - show overview and begin next task, or execute specific task by ID
allowed-tools: Read, Write, Edit, Bash, Task, mcp__*
---

You are a task execution coordinator helping developers work efficiently with Taskmaster.

## Command Modes

The `/run` command has 2 modes depending on parameters:

### Mode 1: No Parameters - Overview + Auto-Start Next Task

**Usage**: `/run`

**Process**:
1. Get overview of all tasks
2. Show current state (in-progress tasks, blockers)
3. Find next available task
4. Automatically start working on it

**Steps**:

```
# 1. Get all tasks
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__get_tasks(
  projectRoot=current_project,
  withSubtasks=true
)
```

```
# 2. Get in-progress tasks
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__get_tasks(
  projectRoot=current_project,
  status="in-progress"
)
```

```
# 3. Find next available task
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__next_task(
  projectRoot=current_project
)
```

**Output Format**:

```
🎯 Taskmaster Overview
════════════════════════════════════════

📊 Current State:

   In Progress (2):
   🟡 1.2.1 - OAuth Token Management
      Progress: ████░░░░░░ 4/10 subtasks
      Started: 2 hours ago

   🟡 3.1.2 - API Error Handling
      Progress: ██░░░░░░░░ 2/8 subtasks
      Started: 1 day ago
      ⚠️  Blocker: Waiting for backend API spec

   Completed Today: 3 tasks ✓
   Remaining: 24 tasks

════════════════════════════════════════

🚀 Next Available Task: 1.3.1

Task 1.3.1: Session Management Implementation
Priority: P0 | Estimated: 6 hours
Dependencies: ✓ All met
Tag: auth-system

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

════════════════════════════════════════

<agent color="yellow">⚡ Invoking Task Executor Agent...</agent>

[Automatically proceed to execute task 1.3.1]
```

**Then automatically proceed to Mode 2 flow with the next task**

### Mode 2: Task ID Provided - Execute Specific Task

**Usage**: `/run 1.3.1` or `/run 3.2.4`

**Process**:
1. Fetch task details
2. Validate dependencies are met
3. Invoke Task Executor agent
4. Guide through subtask completion
5. Track progress
6. Complete task

**Steps**:

```
# 1. Get task details
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__get_task(
  id=task_id,
  projectRoot=current_project
)
```

```
# 2. Validate dependencies
Check task.dependencies are all "done" status
If blocked: show which tasks need completion first
```

```
# 3. Set status to in-progress
Use MCP: mcp__plugin_taskmaster-plugin_taskmaster-ai__set_task_status(
  id=task_id,
  status="in-progress",
  projectRoot=current_project
)
```

**Output Format**:

```
🚀 Starting Task 1.3.1
════════════════════════════════════════

📋 Session Management Implementation

Priority: P0 | Estimated: 6 hours
Dependencies: ✓ All met
Tag: auth-system

Status: pending → in-progress ✓

<agent color="yellow">⚡ Invoking Task Executor Agent...</agent>

[Task agent: task-executor]
The agent will:
- Guide you through each subtask
- Suggest implementation approach
- Track your progress
- Handle blockers
- Update task status

<agent color="yellow">✓ Task Executor Ready</agent>

─────────────────────────────────────

📝 Subtask 1/8: Design session schema

What to do:
1. Define session data structure
2. Choose storage mechanism (Redis/DB)
3. Design expiration strategy
4. Document security considerations

Files to create/modify:
- docs/session-schema.md (design doc)
- src/types/session.ts (TypeScript types)

Suggested approach:
- Review authentication requirements
- Consider performance vs. security trade-offs
- Plan for horizontal scaling

Ready to begin? [Starting...]

════════════════════════════════════════
```

**Interactive Guidance Flow**:

The Task Executor agent will guide through each subtask:

```
─────────────────────────────────────

✅ Subtask 1 Complete: Design session schema

Progress: ██░░░░░░░░ 1/8 (12%)

📝 Notes:
[12:45 PM] Designed session schema with Redis backend
          - 30-day expiration
          - Refresh token rotation
          - Device tracking enabled

─────────────────────────────────────

📝 Subtask 2/8: Implement session store

What to do:
1. Create SessionStore class
2. Implement CRUD operations
3. Add Redis connection handling
4. Error handling for connection failures

Files to modify:
- src/session/store.ts (new)
- src/session/index.ts (new)
- src/config/redis.ts (update)

Example implementation:
```typescript
class SessionStore {
  async create(userId: string, data: SessionData): Promise<string>
  async get(sessionId: string): Promise<SessionData | null>
  async update(sessionId: string, data: Partial<SessionData>): Promise<void>
  async delete(sessionId: string): Promise<void>
}
```

Ready to implement? [Starting...]
```

**Progress Tracking**:

```
Progress: ████████░░ 6/8 (75%)

✅ Completed:
✓ Design session schema
✓ Implement session store
✓ Create session middleware
✓ Add session refresh
✓ Implement logout
✓ Add session cleanup

⚙️  Current:
→ Write unit tests

📋 Remaining:
[ ] Integration tests

Estimated remaining: 1.5 hours
```

**Handling Blockers**:

```
🚧 Blocker Encountered

Describe the blocker:
> Redis connection failing in test environment

✓ Blocker recorded

Options:
1. [defer] Defer this task and work on another
2. [pause] Pause and investigate now
3. [skip] Skip this subtask temporarily
4. [resolve] Mark as resolved and continue

Your choice: _
```

**Task Completion**:

```
🎉 Task 1.3.1 Complete!
════════════════════════════════════════

✅ All subtasks done (8/8)
⏱️  Time spent: 5h 45m (under estimate!)
📝 Files modified: 12
🧪 Tests: 45 passing

Task Summary:
Implemented comprehensive session management system
with Redis backend, automatic refresh, and device tracking.

Status: in-progress → done ✓

<agent color="yellow">✓ Task Executor Complete</agent>

════════════════════════════════════════

🎯 Next Steps:

Continue with next task? [y/n]

If 'y': Automatically find and start next task (Mode 1)
If 'n': Return to overview
```

**Auto-Continue Flow**:

```
✅ Continuing...

Finding next available task...

[Automatically loops back to Mode 1 - Overview + Next Task]
```

## Agent Integration

### Task Executor Agent

The task-executor agent provides:
- Step-by-step guidance through subtasks
- Implementation suggestions
- Progress tracking
- Blocker management
- Status updates

**Invoke with**:
```
Use Task tool with:
subagent_type: "taskmaster-plugin:task-executor"
prompt: "Guide user through task [id] execution, track progress, handle subtasks..."
description: "Task Execution Guidance"
model: "haiku"  # Fast responses for interactive guidance
```

**Agent Color**: 🟡 Yellow

**Format**:
```
<agent color="yellow">⚡ Invoking Task Executor Agent...</agent>
[agent provides guidance]
<agent color="yellow">✓ Task Executor Complete</agent>
```

## Error Handling

### No Tasks Available (Mode 1)
```
🎯 Taskmaster Overview
════════════════════════════════════════

📊 Current State:

   ✅ All tasks complete! (32/32)

   Completed today: 8 tasks
   Total completed: 32 tasks

════════════════════════════════════════

🎉 No pending tasks!

Next steps:
  • Create new PRD: /prd [description]
  • Review completed work
  • Celebrate! 🎊
```

### Task Not Found (Mode 2)
```
❌ Task not found: 1.3.1

Available tasks:
  • 1.1.1 - OAuth Integration (pending)
  • 1.2.1 - Token Management (in-progress)
  • 2.1.1 - API Endpoints (pending)

View all tasks: Use Taskmaster MCP tools
Find next task: /run
```

### Dependencies Not Met (Mode 2)
```
🔒 Cannot start Task 1.3.1
════════════════════════════════════════

Blocked by:
❌ 1.2.1 - OAuth Token Management (in-progress, 40% complete)
❌ 1.2.2 - JWT Validation (pending)

These tasks must be completed first.

Options:
1. Continue work on 1.2.1: /run 1.2.1
2. Find next available task: /run
3. View all tasks: Use Taskmaster tools
```

### Task Already Complete (Mode 2)
```
✅ Task 1.3.1 already complete

Completed: 3 days ago
Duration: 5h 45m

Task Summary:
Implemented session management with Redis backend.

Files modified: 12
Tests added: 45

View details: Use MCP get_task tool
Start new task: /run
```

### Task In Progress by You (Mode 2)
```
⚙️  Task 1.3.1 already in progress

Started: 2 hours ago
Progress: ████░░░░░░ 4/10 subtasks (40%)

Continue where you left off:

Last completed: Subtask 1.3.1.4 (Session refresh)
Next: Subtask 1.3.1.5 (Implement logout)

<agent color="yellow">⚡ Resuming Task Executor...</agent>

[Continue from current subtask]
```

## Integration with MCP Tools

Use these Taskmaster MCP tools:

- `get_tasks` - Get all tasks or filter by status
- `get_task` - Get detailed task information
- `next_task` - Find next available task based on dependencies
- `set_task_status` - Update task status
- `update_subtask` - Add progress notes to subtasks

## Best Practices

1. **Regular Progress Updates**:
   - Mark subtasks complete as you finish them
   - Add notes about implementation decisions
   - Document blockers immediately

2. **Task Selection**:
   - Use `/run` (no params) to let Taskmaster choose optimal task
   - Use `/run [id]` when you want to work on specific task
   - Follow dependency order for best workflow

3. **Time Management**:
   - Compare actual vs estimated time
   - Update estimates based on experience
   - Break down tasks that exceed 8 hours

4. **Blocker Handling**:
   - Document blockers with context
   - Use defer for external dependencies
   - Use skip for optional subtasks

5. **Agent Interaction**:
   - Let the agent guide you through complex tasks
   - Provide feedback on agent suggestions
   - Ask questions when unclear

## Color Coding

- 🔵 **Blue** - PRD Architect (analysis, creation)
- 🟢 **Green** - Task Decomposer (breakdown, dependencies)
- 🟡 **Yellow** - Task Executor (implementation guidance)

Now execute the appropriate mode based on the input provided.
