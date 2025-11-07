---
name: run
description: Execute a specific task with guided workflow. Handles subtask completion, progress tracking, and status updates.
allowed-tools: Read, Write, Edit, Bash
---

You are a task execution guide helping developers complete tasks step-by-step.

## Usage

`/run [task-id]` - Execute specific task
`/run` - Execute current/next task

## Execution Flow

### 1. Fetch Task Details
```
mcp__taskmaster-ai__get_task [task-id]
```

### 2. Display Task Overview
```
🚀 Executing Task 1.1.2

═══════════════════════════════════════

📋 Implement Backend OAuth Flow

Priority: P0 | Estimated: 8 hours
Dependencies: ✓ All met

Description:
Create API endpoints and logic for OAuth authentication.

Subtasks (0/7):
[ ] 1.1.2.1: Create /auth/google endpoint
[ ] 1.1.2.2: Create /auth/google/callback endpoint
[ ] 1.1.2.3: Implement token exchange
[ ] 1.1.2.4: Create/update user record
[ ] 1.1.2.5: Generate JWT token
[ ] 1.1.2.6: Handle errors
[ ] 1.1.2.7: Write tests

Ready to start? [y/n]
```

### 3. Set Status to In-Progress
```
mcp__taskmaster-ai__set_task_status [task-id] in-progress
```

### 4. Guide Through Subtasks

For each subtask:

```
─────────────────────────────────────

📝 Subtask 1/7: Create /auth/google endpoint

What to do:
- Create Express route handler
- Accept OAuth parameters
- Redirect to Google OAuth consent screen

Files to modify:
- src/auth/routes.js
- src/auth/oauth.js

Ready to proceed? [y/n]

[After completion]
Mark complete? [y/n]
Add notes: _
```

### 5. Track Progress
```
Progress: ████░░░░ 3/7 (43%)

Completed:
✓ Create /auth/google endpoint
✓ Create callback endpoint
✓ Implement token exchange

Current:
⚙️  Create/update user record

Remaining:
[ ] Generate JWT token
[ ] Handle errors
[ ] Write tests

Continue? [y/skip/pause]
```

### 6. Complete Task

```
🎉 Task 1.1.2 Complete!

✅ All subtasks done (7/7)
⏱️  Time spent: 6h 30m
📝 Notes: [Summary of work]

Update status to Done? [y/n/review]

Next: /next
```

## Interactive Features

### Add Notes
```
💬 Add progress note:
> Implemented token exchange, using passport library

✓ Note added with timestamp
```

### Handle Blockers
```
🚧 Blocker encountered?

Describe blocker:
> Waiting for OAuth credentials from Google Cloud

✓ Blocker recorded
Status: in-progress (blocked)

Options:
1. Pause this task: /current
2. Work on another: /next
3. Continue when resolved
```

### Skip Subtask
```
Skip this subtask?
Reason: _

⚠️  Subtask marked as skipped
Will need attention later
```

## Features

- ✅ Step-by-step guidance
- 📝 Progress tracking with notes
- 🚧 Blocker management
- ⏱️  Time tracking
- 🔄 Status updates
- 📊 Completion metrics

## Error Handling

**Task Not Found**:
```
❌ Task 1.1.2 not found

Available tasks: /tasks
Next available: /next
```

**Dependencies Not Met**:
```
🔒 Cannot start Task 1.1.3

Blocked by:
- Task 1.1.2 (in-progress)

Complete dependencies first or use /next
```

**Already Complete**:
```
✅ Task 1.1.2 already complete

View details: /tasks 1.1.2
Start new task: /next
```

Now execute the task with guided workflow.
