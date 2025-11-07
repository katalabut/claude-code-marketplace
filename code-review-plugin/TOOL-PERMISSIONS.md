# Tool Permissions Configuration

This document explains how tool permissions are configured in the Code Review Plugin.

## Overview

Claude Code uses a permission-based architecture where access to tools must be explicitly granted. This plugin uses different tools for different components.

## Tool Types

### Available Tools

- **Read** - Read files from the filesystem
- **Edit** - Edit existing files
- **Write** - Create new files
- **Grep** - Search for patterns in files
- **Glob** - Find files by pattern matching
- **Bash** - Execute bash commands (with restrictions)

### Bash Command Restrictions

For security, bash commands can be restricted to specific patterns:
- `Bash(git:*)` - Allow all git commands
- `Bash(gh:*)` - Allow all GitHub CLI commands
- `Bash(git status:*)` - Allow only specific git commands
- `Bash` - Allow all bash commands (not recommended)

## Permission Configuration by Component

### Commands (`commands/review.md`)

**Parameter name:** `allowed-tools`

**Syntax:**
```yaml
allowed-tools: Read, Grep, Glob, Bash(git:*), Bash(gh:*)
```

**Current configuration:**
- `Read` - Read files to analyze code
- `Grep` - Search for patterns in codebase
- `Glob` - Find files by patterns
- `Bash(git:*)` - Execute git commands (status, diff, log, etc.)
- `Bash(gh:*)` - Execute GitHub CLI commands (pr view, pr diff, etc.)

**Why these tools:**
The `/review` command needs to:
- Read files to analyze code
- Use git to see changes (`git diff`, `git status`)
- Use GitHub CLI for PR reviews (`gh pr view`, `gh pr diff`)

### Agents (`agents/*.md`)

**Parameter name:** `tools` (not `allowed-tools`)

**Syntax:**
```yaml
tools: Read, Grep, Glob, Bash, Edit
```

#### code-reviewer Agent

```yaml
tools: Read, Grep, Glob, Bash, Edit
```

**Why these tools:**
- `Read` - Read code files
- `Grep` - Search for patterns across codebase
- `Glob` - Find files matching patterns
- `Bash` - Execute git commands to see changes
- `Edit` - Suggest code fixes (optional)

#### security-expert Agent

```yaml
tools: Read, Grep, Glob, Bash
```

**Why these tools:**
- `Read` - Read files to check for vulnerabilities
- `Grep` - Search for security patterns (passwords, API keys)
- `Glob` - Find configuration and sensitive files
- `Bash` - Run security scanning tools (if available)

#### performance-analyzer Agent

```yaml
tools: Read, Grep, Glob, Bash
```

**Why these tools:**
- `Read` - Read code to analyze performance
- `Grep` - Find performance patterns
- `Glob` - Locate performance-critical files
- `Bash` - Run profiling tools (if available)

**Note:** Agents use general `Bash` access without restrictions. If you want to restrict bash commands for agents, you can use the same syntax as commands:
```yaml
tools: Read, Grep, Glob, Bash(git:*), Bash(gh:*)
```

### Skills (`skills/*/SKILL.md`)

**Parameter name:** `allowed-tools`

**Syntax:**
```yaml
allowed-tools: Read, Grep, Bash(gh:*), Bash(git:*)
```

#### pr-reviewer Skill

```yaml
allowed-tools: Read, Grep, Bash(gh:*), Bash(git:*)
```

**Why these tools:**
- `Read` - Read PR files
- `Grep` - Search PR content
- `Bash(gh:*)` - GitHub CLI for PR operations
- `Bash(git:*)` - Git commands for diff analysis

#### code-quality Skill

```yaml
allowed-tools: Read, Grep, Glob
```

**Why these tools:**
- `Read` - Read code files for analysis
- `Grep` - Search for code patterns
- `Glob` - Find files by language extensions

**No Bash needed:** This skill only analyzes code statically without executing external commands.

## Default Behavior

If `allowed-tools` or `tools` is **omitted**, the component inherits all tools from the parent context (conversation settings).

## Security Best Practices

1. **Principle of Least Privilege**: Grant only the tools needed for the task
2. **Restrict Bash Commands**: Use `Bash(command:*)` syntax to limit command scope
3. **Read-Only When Possible**: Prefer `Read, Grep, Glob` over `Edit, Write` if modifications aren't needed
4. **Audit Tool Usage**: Review what commands are actually being executed

## User-Level Permissions

Users can also configure global permissions in their Claude Code settings:

### Project Settings (`.claude/settings.local.json`)

```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "WebFetch(domain:github.com)",
      "Bash(git:*)",
      "Bash(gh:*)"
    ],
    "deny": [],
    "ask": []
  }
}
```

### Global Settings

Located in Claude Code user settings, this controls which tools are available globally.

## Troubleshooting

### "Tool not available" errors

If you see errors about tools not being available:

1. **Check component configuration**: Verify `allowed-tools` or `tools` includes the required tool
2. **Check user permissions**: Ensure your `.claude/settings.local.json` allows the tool
3. **Check global settings**: Verify global Claude Code settings permit the tool

### GitHub CLI not working

If `gh` commands fail:

1. **Install GitHub CLI**: `brew install gh` (macOS) or see https://cli.github.com/
2. **Authenticate**: Run `gh auth login`
3. **Verify permissions**: Check that `Bash(gh:*)` is in `allowed-tools`

### Git commands not working

If git commands fail:

1. **Verify git is installed**: `git --version`
2. **Check repository**: Ensure you're in a git repository
3. **Verify permissions**: Check that `Bash(git:*)` is in `allowed-tools`

## Updating Permissions

To modify permissions for a component:

1. Open the component file (command, agent, or skill)
2. Edit the `allowed-tools` or `tools` frontmatter field
3. Save the file
4. Reload Claude Code (or restart)

## Examples

### Restricting to read-only operations

```yaml
---
name: safe-reviewer
allowed-tools: Read, Grep, Glob
---
```

### Allowing specific git commands only

```yaml
---
name: git-status-checker
allowed-tools: Read, Bash(git status:*), Bash(git log:*)
---
```

### Full access for complex tasks

```yaml
---
name: powerful-agent
tools: Read, Edit, Write, Grep, Glob, Bash
---
```

## References

- [Claude Code Tools Documentation](https://code.claude.com/docs/en/tools)
- [Skills Documentation](https://code.claude.com/docs/en/skills.md)
- [Sub-agents Documentation](https://code.claude.com/docs/en/sub-agents.md)
- [Slash Commands Documentation](https://code.claude.com/docs/en/slash-commands.md)
