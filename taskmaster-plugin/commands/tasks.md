---
name: tasks
description: List all tasks with filtering, sorting, and hierarchy view. Supports status, priority, and tag filters.
allowed-tools: Read
---

You are a task list manager displaying project tasks.

## Usage

```bash
/tasks                    # List all tasks
/tasks status:pending     # Filter by status
/tasks priority:P0        # Filter by priority
/tasks tag:backend        # Filter by tag
/tasks 1.1.2             # Show specific task
```

## Display Format

### Default View (All Tasks)

```
📋 All Tasks

═══════════════════════════════════════

Theme 1: User Authentication & Security (9 tasks)

Epic 1.1: OAuth 2.0 Authentication
├── ✅ 1.1.1: Set up OAuth Config (P0, done)
├── ⚙️  1.1.2: Implement Backend Flow (P0, in-progress, 57%)
├── ⏸️  1.1.3: Frontend OAuth Button (P0, pending)
└── ⏸️  1.1.4: Testing & Docs (P1, pending)

Epic 1.2: Multi-Factor Authentication
├── ⏸️  1.2.1: TOTP Generation (P1, pending)
├── ⏸️  1.2.2: Backup Codes (P1, pending)
└── ⏸️  1.2.3: MFA UI (P2, pending)

Epic 1.3: Session Management
├── ⏸️  1.3.1: Refresh Tokens (P1, pending)
└── ⏸️  1.3.2: Session Cleanup (P2, pending)

─────────────────────────────────────

📊 Summary:
Total: 9 tasks
✅ Done: 1 (11%)
⚙️  In Progress: 1 (11%)
⏸️  Pending: 7 (78%)

By Priority:
P0: 4 tasks | P1: 4 tasks | P2: 1 task
```

### Filtered View

```bash
/tasks status:pending priority:P0
```

```
📋 Pending P0 Tasks

Found 3 tasks

⏸️  1.1.3: Frontend OAuth Button
   Dependencies: Task 1.1.2 (in-progress)
   Estimated: 6h

⏸️  1.1.4: Testing & Documentation
   Dependencies: Task 1.1.3
   Estimated: 4h

⏸️  2.1.1: Database Schema Design
   Dependencies: None (Ready!)
   Estimated: 8h

💡 Suggested: Task 2.1.1 (no dependencies)
```

### Specific Task

```bash
/tasks 1.1.2
```

```
📋 Task 1.1.2: Implement Backend OAuth Flow

Status: ⚙️  In Progress (57% complete)
Priority: P0 (Critical)
Assignee: @developer1
Started: 2025-01-07 10:30 AM
Estimated: 8h | Spent: 2h 15m

Description:
Create API endpoints and logic for OAuth authentication flow.

Dependencies:
✅ 1.1.1: Set up OAuth Configuration (Done)

Blocks:
🔒 1.1.3: Frontend OAuth Button
🔒 1.1.4: Testing & Documentation

Subtasks (4/7 complete):
✅ 1.1.2.1: Create /auth/google endpoint
✅ 1.1.2.2: Create callback endpoint
✅ 1.1.2.3: Token exchange logic
✅ 1.1.2.4: Create/update user
⚙️  1.1.2.5: Generate JWT (current)
⏸️  1.1.2.6: Handle errors
⏸️  1.1.2.7: Write tests

Recent Notes:
[12:45 PM] JWT generation working, adding refresh tokens
[11:30 AM] Token exchange complete
[10:45 AM] Callback endpoint created

Continue: /run 1.1.2
```

## Filter Options

### By Status
- `status:pending`
- `status:in-progress`
- `status:review`
- `status:done`
- `status:deferred`
- `status:cancelled`

### By Priority
- `priority:P0` (Critical)
- `priority:P1` (High)
- `priority:P2` (Medium)
- `priority:P3` (Low)

### By Tag
- `tag:backend`
- `tag:frontend`
- `tag:testing`
- `tag:docs`
- Custom tags

### Combined Filters
```bash
/tasks status:pending priority:P0 tag:backend
```

## Sort Options

```bash
/tasks --sort priority    # By priority
/tasks --sort status      # By status
/tasks --sort time        # By estimated time
/tasks --sort id          # By task ID (default)
```

## View Options

### Flat List
```bash
/tasks --flat
```
```
1. ✅ 1.1.1: Set up OAuth Config (P0, done)
2. ⚙️  1.1.2: Implement Backend Flow (P0, in-progress)
3. ⏸️  1.1.3: Frontend OAuth Button (P0, pending)
...
```

### Tree View (Default)
Shows hierarchical structure with themes → epics → tasks

### Kanban Style
```bash
/tasks --kanban
```
```
┌─ Pending ──┬─ In Progress ─┬─ Review ─┬─ Done ───┐
│ Task 1.1.3 │ Task 1.1.2    │          │ Task 1.1.1│
│ Task 1.1.4 │               │          │           │
│ Task 1.2.1 │               │          │           │
└────────────┴───────────────┴──────────┴───────────┘
```

## Quick Stats

```
📊 Project Statistics

Total Tasks: 9
Progress: ████░░░░░░ 11% complete

Status Distribution:
✅ Done:        █░░░░░░░░░ 1  (11%)
⚙️  In Progress: █░░░░░░░░░ 1  (11%)
⏸️  Pending:     ████████░░ 7  (78%)

Priority Breakdown:
🔴 P0: 4 tasks (44%)
🟡 P1: 4 tasks (44%)
🟢 P2: 1 task (11%)

Estimated Time:
Total: 54 hours
Remaining: 48 hours
On Track: Yes ✓
```

## Error Handling

**No Tasks**: `/tasks` with no results
```
📋 No Tasks Found

Create tasks:
1. /prd create "Feature description"
2. /prd parse docs/prd/your-prd.md

Or add manually via Taskmaster MCP
```

**Invalid Filter**:
```
❌ Invalid filter: status:invalid

Valid status values:
- pending
- in-progress
- review
- done
- deferred
- cancelled
```

Now display the task list with requested filters.
