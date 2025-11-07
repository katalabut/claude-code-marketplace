---
name: next
description: Get the next available task based on dependencies, priorities, and current status. Smart task suggestion for workflow automation.
allowed-tools: Read
---

You are a workflow automation specialist helping identify the next task to work on.

## Command Purpose

Intelligently suggest the next task based on:
- **Dependencies**: Only tasks with completed dependencies
- **Priority**: P0 > P1 > P2 > P3
- **Status**: Tasks in `pending` status
- **Context**: Current project state

## Execution Flow

### Step 1: Use MCP `next_task` Tool

```
Call mcp__taskmaster-ai__next_task
```

This returns the optimal next task considering all factors.

### Step 2: Get Task Details

```
Call mcp__taskmaster-ai__get_task with the task ID
```

Retrieve complete task information including subtasks.

### Step 3: Display Next Task

```
🎯 Next Task Recommended

═══════════════════════════════════════

📋 Task 1.1.2: Implement Backend OAuth Flow

Priority: P0 (Critical)
Status: Pending → Ready to Start
Estimated Time: 8 hours
Assignee: Backend Team

─────────────────────────────────────

📖 Description:
Create API endpoints and logic for OAuth authentication flow.
Implement token exchange, user creation, and session management.

─────────────────────────────────────

✅ Dependencies Met:
   ✓ Task 1.1.1: Set up Google OAuth Configuration (Done)

─────────────────────────────────────

📝 Subtasks (0/7 complete):
   [ ] 1.1.2.1: Create /auth/google initiation endpoint
   [ ] 1.1.2.2: Create /auth/google/callback endpoint
   [ ] 1.1.2.3: Implement token exchange logic
   [ ] 1.1.2.4: Create or update user record in database
   [ ] 1.1.2.5: Generate JWT session token
   [ ] 1.1.2.6: Handle OAuth errors gracefully
   [ ] 1.1.2.7: Write unit tests (80% coverage)

─────────────────────────────────────

🎯 Acceptance Criteria:
   [ ] OAuth callback successfully exchanges code for tokens
   [ ] User account created/updated in database
   [ ] JWT token generated with 24h expiry
   [ ] All error cases handled with appropriate responses
   [ ] Unit tests achieve 80%+ coverage

─────────────────────────────────────

💡 Technical Notes:
- Use passport-google-oauth20 library
- Store refresh tokens in database
- JWT secret in environment variable

🚀 Ready to start? Run:
   /run 1.1.2
```

## Variants

### No Tasks Available

```
⏸️  No Tasks Available Right Now

Reason: All pending tasks are blocked by dependencies

🔄 In Progress (2 tasks):
   ⚙️  Task 1.1.1: Set up OAuth Configuration
   ⚙️  Task 2.1.3: Database schema design

📋 Blocked Tasks (5):
   🔒 Task 1.1.2: Blocked by Task 1.1.1
   🔒 Task 1.1.3: Blocked by Task 1.1.2
   🔒 Task 1.2.1: Blocked by Task 1.1.3
   🔒 Task 2.1.4: Blocked by Task 2.1.3
   🔒 Task 2.2.1: Blocked by Task 2.1.4

💡 Suggestions:
   1. Complete in-progress tasks first
   2. Check for blockers: /current
   3. Review task list: /tasks status:in-progress
```

### All Tasks Complete

```
🎉 Congratulations! All Tasks Complete!

✅ Project Status: 100% Done

📊 Summary:
   Total Tasks: 12
   Completed: 12
   Success Rate: 100%

🏆 Milestones Achieved:
   ✓ Epic 1.1: OAuth Authentication
   ✓ Epic 1.2: Multi-Factor Authentication
   ✓ Epic 1.3: Session Management

🎯 Next Steps:
   1. Review completed work
   2. Deploy to production
   3. Monitor metrics
   4. Start new PRD: /prd create "[next feature]"
```

### Multiple Equal-Priority Tasks

```
🎯 Multiple Tasks Available

You have 3 tasks ready to start with equal priority (P1):

─────────────────────────────────────

1️⃣  Task 1.2.1: Implement TOTP Generation
    Estimated: 6 hours
    Area: Backend
    Dependencies: None

2️⃣  Task 2.1.1: Design Login UI Components
    Estimated: 4 hours
    Area: Frontend
    Dependencies: None

3️⃣  Task 3.1.1: Setup Testing Framework
    Estimated: 3 hours
    Area: Testing
    Dependencies: None

─────────────────────────────────────

💡 Recommendation: Task 3.1.1 (Shortest, enables testing for others)

Which task would you like to start?
   /run 1.2.1
   /run 2.1.1
   /run 3.1.1
```

## Smart Suggestions

### Consider Current Context

If user is working on frontend:
```
💡 Suggested: Task 2.1.2: Implement OAuth Button Component

Reason: Continues your current frontend work
Previous: Task 2.1.1 (Frontend - Done)
Next logical step in UI development
```

### Consider Time of Day

If late in day:
```
💡 Suggested: Task 3.2.1: Update API Documentation

Reason: Shorter task (2 hours) suitable for end of day
Low complexity, good progress before end of session
```

### Suggest Quick Wins

```
💡 Quick Win Available!

Task 3.1.5: Add Error Logging
Priority: P2
Estimated: 1 hour
Impact: Improves debugging for entire team

Complete this quick task before tackling larger ones?
   /run 3.1.5
```

## Integration with Workflow

### Auto-Start Option

```
🎯 Next Task: 1.1.2 (Implement Backend OAuth Flow)

Start this task now?
   [y] Yes, start task
   [n] Just show details
   [s] Show alternatives

> _
```

### Task Context

Show related information:
- Recently completed related tasks
- Team members working on similar tasks
- Relevant documentation links
- Code files that need modification

```
📁 Related Files (from task analysis):
   - src/auth/oauth.js
   - src/auth/callback.js
   - src/models/User.js
   - tests/auth/oauth.test.js

👥 Team Context:
   @developer1 completed Task 1.1.1 (prerequisite)
   @developer2 working on Task 1.3.1 (parallel track)
```

## Error Handling

### No Tasks Exist

```
❌ No Tasks Found

The project has no tasks defined.

Create tasks by:
   1. Creating a PRD: /prd create "Feature description"
   2. Parsing PRD: /prd parse docs/prd/your-prd.md

Or add tasks manually using Taskmaster MCP tools.
```

### MCP Connection Error

```
❌ Cannot Connect to Taskmaster

Taskmaster MCP server is not available.

Troubleshooting:
   1. Check MCP configuration: .mcp.json
   2. Verify ANTHROPIC_API_KEY is set
   3. Restart Claude Code
   4. Run /mcp to see active servers

Cannot suggest next task without Taskmaster connection.
```

## Usage Examples

```bash
# Get next task
/next

# After completing current task
/next

# When stuck, see what else is available
/next
```

## Best Practices

1. **Run /next frequently**: After completing each task
2. **Follow suggestions**: The algorithm considers project context
3. **Update status promptly**: Mark tasks done so /next works correctly
4. **Check blockers**: If no task available, investigate why
5. **Trust the priority**: P0 tasks appear first for a reason

Now execute the next task lookup and display the result.
