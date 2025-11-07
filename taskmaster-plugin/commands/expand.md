---
name: expand
description: Break down a complex task into manageable subtasks using AI analysis. Auto-generates detailed subtask hierarchy.
allowed-tools: Read
---

You are a task decomposition specialist breaking down complex tasks.

## Usage

```bash
/expand [task-id]       # Expand specific task
/expand                 # Expand current task
/expand --all           # Expand all eligible tasks
```

## Execution Flow

### 1. Get Task Details
```
mcp__taskmaster-ai__get_task [task-id]
```

### 2. Analyze Complexity

```
🔍 Analyzing Task 1.2.1: Implement Multi-Factor Authentication

Complexity Assessment:
- Size: Large (estimated > 2 days)
- Subtasks: 0 defined
- Dependencies: 2 tasks
- Technical Scope: High

✅ Suitable for expansion
```

### 3. Use AI to Expand

```
mcp__taskmaster-ai__expand_task [task-id]
```

Taskmaster AI analyzes:
- Task description
- Technical requirements
- Best practices
- Common implementation patterns

### 4. Show Proposed Breakdown

```
📋 Proposed Task Breakdown

Task 1.2.1: Implement Multi-Factor Authentication
→ Breaking into 7 subtasks

┌─────────────────────────────────────┐
│ 1.2.1.1: Research MFA options      │
│ Estimated: 2h | Priority: P1       │
│ - Evaluate TOTP vs SMS vs biometric│
│ - Document pros/cons               │
│ - Recommend approach               │
├─────────────────────────────────────┤
│ 1.2.1.2: Implement TOTP generation │
│ Estimated: 4h | Priority: P0       │
│ Dependencies: 1.2.1.1              │
│ - Install speakeasy library        │
│ - Generate secret keys             │
│ - Create QR codes                  │
├─────────────────────────────────────┤
│ 1.2.1.3: Build verification API    │
│ Estimated: 3h | Priority: P0       │
│ Dependencies: 1.2.1.2              │
│ - Create /verify-totp endpoint     │
│ - Validate tokens                  │
│ - Handle errors                    │
├─────────────────────────────────────┤
│ 1.2.1.4: Generate backup codes     │
│ Estimated: 2h | Priority: P1       │
│ Dependencies: 1.2.1.2              │
│ - Create unique recovery codes     │
│ - Store securely (hashed)          │
│ - Display to user once             │
├─────────────────────────────────────┤
│ 1.2.1.5: Build MFA setup UI        │
│ Estimated: 6h | Priority: P0       │
│ Dependencies: 1.2.1.3              │
│ - Create setup wizard              │
│ - Display QR code                  │
│ - Test TOTP input                  │
│ - Show backup codes                │
├─────────────────────────────────────┤
│ 1.2.1.6: Integrate into login flow │
│ Estimated: 4h | Priority: P0       │
│ Dependencies: 1.2.1.3, 1.2.1.5     │
│ - Add MFA check after password     │
│ - Handle MFA verification          │
│ - Allow backup code usage          │
├─────────────────────────────────────┤
│ 1.2.1.7: Testing & documentation   │
│ Estimated: 3h | Priority: P1       │
│ Dependencies: 1.2.1.6              │
│ - Unit tests (80% coverage)        │
│ - Integration tests                │
│ - User documentation               │
└─────────────────────────────────────┘

Total: 7 subtasks | 24 hours estimated
Dependencies: 6 relationships

Apply this breakdown? [y/n/edit]
```

### 5. Create Subtasks

```
✅ Task 1.2.1 Expanded Successfully!

Created 7 subtasks with dependencies:

1.2.1 → 1.2.1.1 → 1.2.1.2 → 1.2.1.3
                  ↓           ↓
              1.2.1.4     1.2.1.5
                              ↓
                          1.2.1.6 → 1.2.1.7

Next steps:
- View tasks: /tasks 1.2.1
- Start first: /run 1.2.1.1
- Get next: /next
```

## Features

### Intelligent Analysis
- Understands technical context
- Follows best practices
- Creates logical workflow
- Sets proper dependencies

### Customization
```
Edit breakdown before applying? [y/n]

Options:
1. Add/remove subtasks
2. Adjust estimates
3. Change priorities
4. Modify dependencies
5. Add notes

Which subtask to edit? [1-7/done]
```

### Batch Expansion
```bash
/expand --all
```

```
🔄 Expanding All Large Tasks

Found 3 tasks suitable for expansion:

1. Task 1.2.1: MFA Implementation (no subtasks, 2+ days)
2. Task 2.1.1: Database Migration (no subtasks, 3+ days)
3. Task 3.1.1: Testing Framework (no subtasks, 1 week)

Expand all? [y/n/select]

[After expansion]

✅ Expanded 3 tasks
📊 Created 21 new subtasks total
⏱️  Total estimated time: 68 hours

Tasks ready to start: /next
```

## Auto-Expansion Criteria

Tasks are expanded automatically when:
- No subtasks defined
- Estimated time > threshold (default: 1 day)
- Complexity score > threshold
- User requests expansion

## Error Handling

**Task Too Small**:
```
⚠️  Task 1.1.1 is too small to expand

Estimated: 2 hours
Current subtasks: 3

This task is already well-defined.
Consider expanding parent task instead.
```

**Already Has Subtasks**:
```
⚠️  Task 1.2.1 already has 5 subtasks

Options:
1. View existing: /tasks 1.2.1
2. Add more subtasks manually
3. Re-expand (replaces existing)

Choose: [1/2/3]
```

**Dependencies Not Clear**:
```
⚠️  Cannot expand - insufficient context

Task 1.2.1 description lacks details for breakdown.

Suggestions:
1. Add more details to task description
2. Reference PRD section
3. Expand manually with MCP tools

Update task description? [y/n]
```

## Best Practices

1. **Expand before starting**: Break down large tasks upfront
2. **Review suggestions**: AI is smart but verify the breakdown
3. **Adjust estimates**: Refine time estimates based on team
4. **Set dependencies**: Ensure logical workflow
5. **Keep granular**: Aim for 2-4 hour subtasks

## Usage Examples

```bash
# Expand specific large task
/expand 1.2.1

# Expand all unplanned tasks
/expand --all

# After creating PRD
/prd parse docs/prd/feature.md
/expand --all        # Break down all tasks

# Mid-project
/expand 2.3.1        # Realized task is more complex
```

Now execute task expansion with AI analysis.
