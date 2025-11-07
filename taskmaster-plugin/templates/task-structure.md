# Task Structure Guide

This guide explains how to structure tasks hierarchically for optimal project management with Taskmaster.

## Hierarchical Breakdown

```
PROJECT
├── THEME (Strategic Initiative - 1-3 months)
│   ├── EPIC (Large Body of Work - 2-4 weeks)
│   │   ├── FEATURE (Specific Functionality - 3-5 days)
│   │   │   ├── TASK (Unit of Work - 1 day)
│   │   │   │   ├── SUBTASK (Implementation Step - 2-4 hours)
│   │   │   │   ├── SUBTASK
│   │   │   │   └── SUBTASK
│   │   │   ├── TASK
│   │   │   └── TASK
│   │   ├── FEATURE
│   │   └── FEATURE
│   ├── EPIC
│   └── EPIC
└── THEME
```

## Level Definitions

### Theme (Strategic Initiative)
- **Duration**: 1-3 months
- **Scope**: Major project initiative or product area
- **Example**: "User Authentication & Security", "Analytics Dashboard"
- **Purpose**: Align work with strategic goals

### Epic (Large Body of Work)
- **Duration**: 2-4 weeks
- **Scope**: Substantial feature set or major improvement
- **Example**: "Implement OAuth 2.0 Authentication", "Build Real-time Notifications"
- **Purpose**: Group related features into deliverable milestones

### Feature (Specific Functionality)
- **Duration**: 3-5 days
- **Scope**: Concrete user-facing capability
- **Example**: "Google OAuth Integration", "Email Notification System"
- **Purpose**: Define specific functionality users interact with

### Task (Unit of Work)
- **Duration**: 1 day (4-8 hours)
- **Scope**: Specific development work
- **Example**: "Implement Backend OAuth Flow", "Create Email Templates"
- **Purpose**: Actionable work item for individual developer

### Subtask (Implementation Step)
- **Duration**: 2-4 hours
- **Scope**: Granular implementation detail
- **Example**: "Create /auth/callback endpoint", "Write unit tests"
- **Purpose**: Detailed checklist for task completion

## Numbering Convention

Use **decimal notation** for hierarchical clarity:

```
1. Theme
  1.1. Epic
    1.1.1. Feature
      1.1.1.1. Task
        1.1.1.1.1. Subtask
        1.1.1.1.2. Subtask
      1.1.1.2. Task
    1.1.2. Feature
  1.2. Epic
2. Theme
```

**Benefits**:
- **Unique IDs**: Each item has a unique identifier
- **Hierarchy visible**: Depth indicated by decimal places
- **Easy reference**: "Task 1.2.3.4" is instantly understood
- **Sortable**: Numeric sorting maintains hierarchy

## Complete Example

```markdown
## 1. Theme: User Authentication & Security

**Goal**: Implement comprehensive authentication system
**Timeline**: 6 weeks
**Priority**: P0 (Critical)

---

### 1.1. Epic: OAuth 2.0 Authentication

**Goal**: Enable secure third-party authentication
**Timeline**: 2 weeks
**Status**: In Progress
**Dependencies**: None

---

#### 1.1.1. Feature: Google OAuth Integration

**Description**: Allow users to sign in with Google accounts
**Effort**: 3 days
**Status**: In Progress
**Assignee**: Backend Team
**Priority**: P0

---

##### 1.1.1.1. Task: Set up Google OAuth Configuration

**Description**: Configure Google Cloud project and obtain OAuth credentials
**Estimated Time**: 4 hours
**Status**: Done
**Assignee**: @developer1
**Dependencies**: None

**Subtasks**:
- [x] 1.1.1.1.1: Create Google Cloud project
- [x] 1.1.1.1.2: Enable Google+ API
- [x] 1.1.1.1.3: Generate OAuth 2.0 credentials
- [x] 1.1.1.1.4: Configure authorized redirect URIs
- [x] 1.1.1.1.5: Store credentials in environment variables

**Notes**:
- Used project ID: `my-app-prod-123`
- Redirect URI: `https://myapp.com/auth/google/callback`

---

##### 1.1.1.2. Task: Implement Backend OAuth Flow

**Description**: Create API endpoints and logic for OAuth authentication
**Estimated Time**: 8 hours
**Status**: In Progress
**Assignee**: @developer1
**Dependencies**: 1.1.1.1

**Subtasks**:
- [x] 1.1.1.2.1: Create `/auth/google` initiation endpoint
- [x] 1.1.1.2.2: Create `/auth/google/callback` endpoint
- [ ] 1.1.1.2.3: Implement token exchange logic
- [ ] 1.1.1.2.4: Create or update user record in database
- [ ] 1.1.1.2.5: Generate JWT session token
- [ ] 1.1.1.2.6: Handle OAuth errors gracefully
- [ ] 1.1.1.2.7: Write unit tests (80% coverage target)

**Technical Details**:
- Use `passport-google-oauth20` library
- JWT expiry: 24 hours
- Refresh token stored in database

**Blockers**:
- None

---

##### 1.1.1.3. Task: Implement Frontend OAuth Button

**Description**: Add "Sign in with Google" button and flow
**Estimated Time**: 6 hours
**Status**: Pending
**Assignee**: @developer2
**Dependencies**: 1.1.1.2

**Subtasks**:
- [ ] 1.1.1.3.1: Add Google OAuth button component
- [ ] 1.1.1.3.2: Implement OAuth popup flow
- [ ] 1.1.1.3.3: Handle callback response
- [ ] 1.1.1.3.4: Store JWT in secure httpOnly cookie
- [ ] 1.1.1.3.5: Redirect to dashboard on success
- [ ] 1.1.1.3.6: Display error messages on failure
- [ ] 1.1.1.3.7: Write E2E tests for happy path
- [ ] 1.1.1.3.8: Write E2E tests for error cases

**Acceptance Criteria**:
- [ ] Button displays Google logo and "Sign in with Google" text
- [ ] Clicking button opens OAuth popup
- [ ] Successful auth redirects to dashboard
- [ ] Failed auth shows clear error message
- [ ] Works on mobile and desktop

---

##### 1.1.1.4. Task: Testing & Documentation

**Description**: Comprehensive testing and user documentation
**Estimated Time**: 4 hours
**Status**: Pending
**Assignee**: @developer1, @developer2
**Dependencies**: 1.1.1.3

**Subtasks**:
- [ ] 1.1.1.4.1: Integration testing
- [ ] 1.1.1.4.2: Security audit
- [ ] 1.1.1.4.3: Write API documentation
- [ ] 1.1.1.4.4: Write user guide
- [ ] 1.1.1.4.5: Update FAQ

---

#### 1.1.2. Feature: GitHub OAuth Integration

**Description**: Allow users to sign in with GitHub accounts
**Effort**: 2 days
**Status**: Pending
**Assignee**: Backend Team
**Priority**: P1
**Dependencies**: 1.1.1 (can reuse OAuth infrastructure)

[Similar structure to 1.1.1]

---

#### 1.1.3. Feature: OAuth Account Linking

**Description**: Allow existing users to link OAuth accounts
**Effort**: 2 days
**Status**: Pending
**Priority**: P1
**Dependencies**: 1.1.1, 1.1.2

[Task breakdown]

---

### 1.2. Epic: Multi-Factor Authentication (MFA)

**Goal**: Add MFA for enhanced security
**Timeline**: 2 weeks
**Status**: Pending
**Dependencies**: 1.1

[Epic breakdown with features and tasks]

---

### 1.3. Epic: Session Management

**Goal**: Robust session handling with refresh tokens
**Timeline**: 1 week
**Status**: Pending
**Dependencies**: 1.1, 1.2

[Epic breakdown]

---

## 2. Theme: Analytics Dashboard

[Similar structure for next theme]
```

## Task Attributes

Each task should include:

### Required Attributes
- **ID**: Unique decimal identifier (e.g., 1.2.3.4)
- **Title**: Clear, action-oriented name
- **Description**: What needs to be done and why
- **Status**: `pending`, `in-progress`, `review`, `done`, `deferred`, `cancelled`
- **Priority**: `P0` (Critical), `P1` (High), `P2` (Medium), `P3` (Low)

### Optional Attributes
- **Estimated Time**: How long it will take
- **Assignee**: Who is responsible
- **Dependencies**: Tasks that must complete first
- **Tags**: Categories (e.g., `backend`, `frontend`, `bug`, `feature`)
- **Due Date**: When it should be completed
- **Subtasks**: Checklist of steps
- **Notes**: Additional context
- **Blockers**: What's preventing progress

## Dependency Management

### Types of Dependencies

**1. Finish-to-Start (Most Common)**
Task B cannot start until Task A finishes.
```
Task 1.1.1 → Task 1.1.2 → Task 1.1.3
```

**2. Start-to-Start**
Task B cannot start until Task A starts (parallel work).
```
Task 1.1.1 (Design) starts → Task 1.1.2 (Documentation) can start
```

**3. Finish-to-Finish**
Task B cannot finish until Task A finishes.
```
Task 1.1.1 (Development) → Task 1.1.2 (Testing) must finish after
```

### Dependency Notation

In Taskmaster:
```markdown
**Dependencies**: 1.1.1, 1.1.2
```

Meaning: This task depends on tasks 1.1.1 and 1.1.2 completing first.

### Avoiding Circular Dependencies

**BAD** (Circular):
```
Task 1 depends on Task 2
Task 2 depends on Task 3
Task 3 depends on Task 1  ← CIRCULAR!
```

**GOOD** (Linear):
```
Task 1 → Task 2 → Task 3
```

## Status Workflow

```
pending → in-progress → review → done
    ↓          ↓
 deferred   cancelled
```

### Status Definitions

- **pending**: Not started, waiting for dependencies or resources
- **in-progress**: Currently being worked on
- **review**: Work complete, awaiting review/approval
- **done**: Completed and approved
- **deferred**: Postponed to future sprint/release
- **cancelled**: No longer needed

## Priority Guidelines

### P0 (Critical)
- **Blocks release**: Must have for MVP/launch
- **High impact**: Affects all users or core functionality
- **Examples**: Authentication system, payment processing, data loss bug

### P1 (High)
- **Important**: Significantly improves user experience
- **High value**: Addresses major pain points
- **Examples**: Search functionality, notifications, mobile responsiveness

### P2 (Medium)
- **Nice to have**: Enhances experience but not critical
- **Moderate value**: Useful but users can work around
- **Examples**: Keyboard shortcuts, dark mode, export features

### P3 (Low)
- **Future consideration**: Minimal impact on current goals
- **Low urgency**: Can be deferred without consequence
- **Examples**: Easter eggs, advanced customization, rare edge cases

## Estimation Guidelines

### T-Shirt Sizing (High Level)
- **XS**: < 2 hours (single subtask)
- **S**: 2-4 hours (few subtasks)
- **M**: 1 day (several subtasks)
- **L**: 2-3 days (complex task with many subtasks)
- **XL**: 1 week (should be broken into multiple tasks)
- **XXL**: > 1 week (must be broken down into epic/features)

### Time Estimation Tips
1. Break down large tasks until each is 1 day or less
2. Include time for testing and documentation
3. Add buffer for unexpected issues (multiply estimate by 1.5)
4. Consider developer experience level
5. Account for dependencies and waiting time

## Best Practices

### DO ✅
- **Use clear, action-oriented titles**: "Implement user login" not "Login stuff"
- **Keep tasks atomic**: Each task should accomplish one clear goal
- **Update status regularly**: Keep task status current
- **Document blockers immediately**: Track what's preventing progress
- **Link related tasks**: Use dependencies to show relationships
- **Break down large tasks**: No task should take more than 1 day

### DON'T ❌
- **Create vague tasks**: "Fix bugs" → Specify which bugs
- **Nest too deeply**: Limit to 5 levels maximum
- **Ignore dependencies**: Track what must happen first
- **Mix concerns**: Keep backend/frontend/testing tasks separate
- **Forget to update**: Stale task data misleads the team
- **Over-engineer**: Don't create tasks for every tiny detail

## Task Templates

### Bug Fix Task Template
```markdown
##### Task: Fix [Bug Description]

**Description**: [Detailed bug description]
**Steps to Reproduce**:
1. [Step 1]
2. [Step 2]
3. [Observed behavior]

**Expected Behavior**: [What should happen]
**Actual Behavior**: [What currently happens]
**Priority**: P0/P1/P2
**Status**: pending

**Subtasks**:
- [ ] Reproduce bug locally
- [ ] Identify root cause
- [ ] Implement fix
- [ ] Add test to prevent regression
- [ ] Verify fix in staging
```

### Feature Task Template
```markdown
##### Task: Implement [Feature Name]

**Description**: [What the feature does and why]
**User Story**: As a [user], I want [goal], so that [benefit]
**Priority**: P0/P1/P2
**Status**: pending

**Acceptance Criteria**:
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

**Subtasks**:
- [ ] Design/planning
- [ ] Implementation
- [ ] Unit tests
- [ ] Integration tests
- [ ] Documentation
```

### Research Task Template
```markdown
##### Task: Research [Topic]

**Description**: [What needs to be researched and why]
**Questions to Answer**:
- [Question 1]
- [Question 2]

**Deliverable**: [Document, POC, recommendation]
**Priority**: P2/P3
**Status**: pending

**Subtasks**:
- [ ] Review existing solutions
- [ ] Evaluate options
- [ ] Create comparison matrix
- [ ] Recommend approach
- [ ] Document findings
```

---

## Taskmaster Integration

### Using with Taskmaster MCP

Taskmaster will automatically parse this structure when you run:

```bash
/prd parse docs/prd/your-prd.md
```

This will:
1. Create tasks with proper IDs
2. Establish dependencies
3. Set up status tracking
4. Enable `/next` to suggest tasks

### Commands for Task Management

```bash
/tasks                  # List all tasks
/next                   # Get next available task
/current                # Show in-progress tasks
/run 1.2.3              # Start working on task 1.2.3
/expand 1.2             # Break down task 1.2 into subtasks
```

---

**Following this structure ensures**:
- Clear project hierarchy
- Logical task flow
- Accurate dependency tracking
- Efficient task management
- Team alignment
