---
name: current
description: Show all currently in-progress tasks with details, progress, and blockers. Track what you're actively working on.
allowed-tools: Read
---

You are a progress tracking specialist showing current work status.

## Command Purpose

Display all tasks currently in progress with:
- Task details and subtask completion
- Progress percentage
- Time tracking
- Blockers and notes
- Next steps

## Execution Flow

### Step 1: Query In-Progress Tasks

```
Call mcp__taskmaster-ai__get_tasks with filter status:in-progress
```

### Step 2: Display Current Work

```
⚙️  Currently In Progress

═══════════════════════════════════════

Found 2 tasks actively being worked on

─────────────────────────────────────

📋 Task 1.1.2: Implement Backend OAuth Flow

Priority: P0 | Started: 2025-01-07 10:30 AM
Assignee: @developer1
Elapsed Time: 2h 15m

Progress: ████████░░░░░░░░ 4/7 subtasks (57%)

✅ Completed:
   ✓ 1.1.2.1: Create /auth/google initiation endpoint
   ✓ 1.1.2.2: Create /auth/google/callback endpoint
   ✓ 1.1.2.3: Implement token exchange logic
   ✓ 1.1.2.4: Create or update user record

🔄 In Progress:
   ⚙️  1.1.2.5: Generate JWT session token (80% done)

⏸️  Pending:
   [ ] 1.1.2.6: Handle OAuth errors
   [ ] 1.1.2.7: Write unit tests

📝 Latest Update (5m ago):
   "JWT generation working, implementing refresh token logic"

🎯 Next Steps:
   - Complete JWT token generation
   - Add error handling
   - Write comprehensive tests

─────────────────────────────────────

📋 Task 2.1.1: Design Login UI Components

Priority: P1 | Started: 2025-01-07 09:00 AM
Assignee: @developer2
Elapsed Time: 4h 00m

Progress: ███████████░░░░░ 5/7 subtasks (71%)

✅ Completed:
   ✓ 2.1.1.1: Create login form component
   ✓ 2.1.1.2: Add OAuth button styles
   ✓ 2.1.1.3: Implement form validation
   ✓ 2.1.1.4: Add loading states
   ✓ 2.1.1.5: Responsive design

⏸️  Pending:
   [ ] 2.1.1.6: Accessibility testing
   [ ] 2.1.1.7: Browser compatibility

📝 Latest Update (15m ago):
   "Form validation complete, starting accessibility audit"

🚧 Blocker:
   Waiting for design team approval on button colors

🎯 Next Steps:
   - Get design approval
   - Complete accessibility testing
   - Test on multiple browsers

─────────────────────────────────────

📊 Overall Status:

   Active Tasks: 2
   Total Subtasks: 14
   Completed: 9 (64%)
   Remaining: 5 (36%)

   Average Progress: 64%
   Total Time Spent: 6h 15m

💡 Suggestions:
   - Task 1.1.2 is 57% done, push to completion
   - Task 2.1.1 has blocker, may need attention
   - No new tasks started - good focus!
```

## Variants

### No In-Progress Tasks

```
✨ No Tasks In Progress

All tasks are either pending or completed.

📋 Available Tasks: 5 pending
✅ Completed Tasks: 3 done
🎯 Suggested: Start next task with /next

Status Distribution:
   Pending: ████████░░ 5 tasks
   Done: ███░░░░░░░ 3 tasks
   Review: ░░░░░░░░░░ 0 tasks

Ready to start? Run: /next
```

### Single Task

```
⚙️  Currently In Progress

═══════════════════════════════════════

📋 Task 1.1.2: Implement Backend OAuth Flow

[Detailed view as above]

─────────────────────────────────────

💡 Tip: Focus on completing this task before starting others
```

### Multiple Tasks with Issues

```
⚠️  Multiple Tasks In Progress - Consider Focus

═══════════════════════════════════════

You have 4 tasks in progress simultaneously.

🚨 Tasks with Blockers (2):
   🔒 Task 2.1.1: Waiting for design approval
   🔒 Task 3.2.1: External API access pending

⚙️  Active Tasks (2):
   ⏳ Task 1.1.2: 57% complete (2h 15m)
   ⏳ Task 1.3.1: 25% complete (45m)

─────────────────────────────────────

💡 Recommendation:
   Focus on completing Task 1.1.2 (nearly done)
   Consider pausing Task 1.3.1 until 1.1.2 is complete

Better focus = faster completion!
```

## Detailed Task View

### With Time Estimates

```
📋 Task 1.1.2: Implement Backend OAuth Flow

⏱️  Time Tracking:
   Estimated: 8 hours
   Spent: 2h 15m
   Remaining: ~5h 45m (estimated)
   On Track: Yes ✓

   Started: 2025-01-07 10:30 AM
   Expected Completion: Today 6:00 PM
```

### With Dependencies

```
📋 Task 1.1.2: Implement Backend OAuth Flow

🔗 Dependencies:
   ✅ Requires: Task 1.1.1 (Complete)
   🚧 Blocks: Task 1.1.3, Task 1.1.4

   If this task is delayed:
   - 2 downstream tasks affected
   - Epic 1.1 timeline at risk
```

### With Notes History

```
📝 Progress Notes:

   [2025-01-07 12:45 PM] (30m ago)
   "JWT generation working, implementing refresh token logic"

   [2025-01-07 11:30 AM] (1h 45m ago)
   "Token exchange complete, starting user creation"

   [2025-01-07 10:45 AM] (2h 30m ago)
   "Callback endpoint created, testing with Google OAuth"

   [2025-01-07 10:30 AM] (2h 45m ago)
   "Started task - creating endpoints"
```

## Filtering and Options

### Filter by Assignee

```bash
/current @developer1
```

```
⚙️  Tasks In Progress: @developer1

Found 1 task

📋 Task 1.1.2: Implement Backend OAuth Flow
[Details...]
```

### Show Detailed View

```bash
/current --detailed
```

Shows expanded information:
- Full subtask list with status
- Complete notes history
- All blockers and dependencies
- Code files affected
- Related PRs/commits

### Show Summary Only

```bash
/current --summary
```

```
⚙️  Current Work Summary

2 tasks in progress
9/14 subtasks complete (64%)
6h 15m total time spent
1 blocker detected

Run `/current` for details
```

## Integration with Other Commands

### Suggest Actions

```
🎯 Suggested Actions:

1. Complete Task 1.1.2 (nearly done):
   /run 1.1.2

2. Address blocker in Task 2.1.1:
   - Contact design team
   - Or defer task: Use MCP set_task_status

3. When complete, get next:
   /next
```

### Update Progress

```
💡 Update progress anytime:

During task execution:
   /run 1.1.2
   → Mark subtasks complete as you go
   → Add notes about progress
   → System tracks automatically

Or use MCP tools:
   mcp__taskmaster-ai__update_subtask
   → Add timestamped progress notes
```

## Error Handling

### No Tasks Found

```
❌ No Tasks In Progress

Either:
   - No tasks have been started yet
   - All tasks are complete or pending

Check all tasks: /tasks
Start next task: /next
```

### MCP Error

```
❌ Cannot Retrieve Task Status

Taskmaster MCP server error.

Try:
   1. Check MCP connection: /mcp
   2. Verify .mcp.json configuration
   3. Restart Claude Code

Cannot display current tasks without Taskmaster.
```

## Best Practices

1. **Check frequently**: Run `/current` multiple times per day
2. **Update notes**: Add progress notes regularly for context
3. **Address blockers**: Don't let blocked tasks sit idle
4. **Limit WIP**: Try to keep 1-2 tasks in progress maximum
5. **Complete before starting new**: Finish tasks before picking up more

## Usage Examples

```bash
# See what you're working on
/current

# Check specific developer's work
/current @username

# Quick summary
/current --summary

# After working for a while
/current  # See progress

# Before ending day
/current  # Document what's left
```

Now execute the current task status display.
