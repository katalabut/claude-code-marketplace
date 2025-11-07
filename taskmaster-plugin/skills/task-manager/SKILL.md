---
name: task-manager  
description: Manages task workflow, suggests next actions, tracks progress automatically. Use when user asks about tasks, progress, "what's next", or task status.
version: 1.0.0
allowed-tools: Read
---

# Task Manager Skill

Autonomous task management and workflow optimization skill.

## Capabilities
- Track all task statuses
- Suggest next available task
- Monitor progress
- Identify blockers
- Manage dependencies
- Report project status

## When to Activate
- User asks "what's next"
- User mentions "tasks" or "progress"
- User wants status update
- User asks what to work on
- User mentions "current work"

## Core Functions

### 1. Next Task Suggestion
Use MCP: `next_task`

Consider:
- Dependencies (completed prerequisites)
- Priority (P0 > P1 > P2 > P3)
- Status (pending only)
- Context (related to recent work)
- Team availability

### 2. Progress Tracking
Use MCP: `get_tasks`

Monitor:
- Tasks by status
- Completion percentage
- Time spent vs estimated
- Blocked tasks
- Upcoming deadlines

### 3. Status Reporting
Generate summaries:
- Overall project progress
- Current sprint/phase status
- Blocker identification
- Velocity metrics
- Completion forecasts

### 4. Workflow Optimization
Suggest:
- Tasks that can run parallel
- Critical path items
- Quick wins
- Blocker resolution strategies

## Output Formats

### Next Task
```
🎯 Next: Task 1.2.1
Priority: P0 | Ready to start
Dependencies: ✓ All met
Estimated: 6h

Start with: /run 1.2.1
```

### Progress Summary
```
📊 Project Status

Progress: ████████░░ 35% (15/45 tasks)
On Track: Yes ✓

This Week:
✅ Completed: 5 tasks
⚙️  In Progress: 2 tasks
📅 Planned: 8 tasks
```

### Blockers Alert
```
🚧 Blockers Detected (2)

Task 2.1.1: Waiting design approval
Task 3.1.5: External API access pending

Recommend: Address before Sprint end
```

## Integration

Coordinates with:
- `/next` command - task suggestions
- `/current` command - active work tracking
- `/tasks` command - task list management
- Taskmaster MCP - data source

Provides autonomous workflow intelligence throughout development process.
