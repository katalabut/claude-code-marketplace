# Code Review Plugin

A comprehensive code review plugin for Claude Code that provides automated analysis of code quality, security vulnerabilities, and documentation. Supports Go, Python, and JavaScript/TypeScript projects with GitHub Pull Request integration.

## Features

### 🔍 Multi-Level Review Capabilities

- **Command-Based Review** (`/review`): User-initiated code reviews with flexible parameters
- **Agent-Based Review**: Proactive, autonomous code review by specialized agents
- **Skill-Based Analysis**: Automatic activation of specialized code analysis capabilities

### 🔒 Security Analysis

- OWASP Top 10 vulnerability detection
- SQL injection, XSS, and command injection detection
- Hardcoded secrets and credential scanning
- Insecure dependency identification
- Authentication and authorization checks

### ✅ Code Quality Checks

- Language-specific best practices (Go, Python, JavaScript/TypeScript)
- Anti-pattern detection
- Code complexity assessment
- Maintainability scoring
- Test coverage evaluation
- Documentation quality verification

### ⚡ Performance Analysis

- Algorithm complexity analysis (Big O notation)
- N+1 query detection
- Memory leak identification
- Concurrency issue detection
- I/O optimization recommendations

### 🔗 GitHub Integration

- Pull Request review with `gh` CLI
- GitHub-compatible review comments
- PR metadata analysis
- CI/CD status verification
- Diff-based code analysis

## Installation

### Prerequisites

- Claude Code installed
- `gh` CLI (GitHub CLI) for PR review functionality
  ```bash
  # macOS
  brew install gh

  # Linux
  # See https://github.com/cli/cli/blob/trunk/docs/install_linux.md

  # Windows
  # See https://github.com/cli/cli/releases
  ```
- Git repository (for local change review)

### Install Plugin

1. Clone or download this plugin repository
2. Add the plugin to your Claude Code plugins directory
3. The plugin will be automatically available in Claude Code

```bash
# Example installation
cd ~/.claude/plugins
git clone <repository-url> code-review-plugin
```

## Usage

### Command: `/review`

The `/review` command is the main entry point for code reviews.

#### Basic Usage

```bash
# Review current uncommitted changes
/review

# Review specific GitHub PR
/review https://github.com/owner/repo/pull/123

# Detailed review mode (includes style checks)
/review --strict

# Security-focused review only
/review --security-only
```

#### Review Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| Standard (default) | Bugs + Security + Important improvements + Documentation | General code review |
| `--strict` | All checks including style, refactoring suggestions | Pre-merge review |
| `--security-only` | Focus exclusively on security vulnerabilities | Security audit |

### Agents

Agents are automatically invoked by Claude Code based on context, or can be manually selected from the `/agents` menu.

#### 1. `code-reviewer` Agent

**Main code review specialist**

- Automatically activated when code is modified
- Coordinates other specialized agents
- Provides comprehensive code quality feedback
- Uses Sonnet model for balanced speed and quality

**Manual Invocation:**
```
Can you review the code I just wrote?
```

#### 2. `security-expert` Agent

**Security vulnerability specialist**

- Deep security analysis using OWASP Top 10 framework
- Identifies injection attacks, authentication issues, data exposure
- Provides detailed security reports with risk levels
- Uses Sonnet model for accuracy

**Manual Invocation:**
```
Can you do a security audit of this authentication code?
```

#### 3. `performance-analyzer` Agent

**Performance optimization specialist**

- Identifies algorithmic inefficiencies
- Detects N+1 queries and memory leaks
- Analyzes concurrency patterns
- Uses Haiku model for fast analysis

**Manual Invocation:**
```
Can you check this code for performance issues?
```

### Skills

Skills are automatically activated by Claude Code when relevant context is detected.

#### 1. `pr-reviewer` Skill

**GitHub Pull Request specialist**

Automatically activates when:
- User mentions "pull request" or "PR"
- User provides a GitHub PR URL
- User asks to review code on GitHub

Capabilities:
- Fetches PR metadata via `gh` CLI
- Analyzes PR diff and changed files
- Generates GitHub-compatible review comments
- Checks CI/CD status

#### 2. `code-quality` Skill

**Language-specific quality analyst**

Automatically activates when:
- Reviewing code files
- Analyzing code structure
- Checking language-specific patterns

Capabilities:
- Go: Idiomatic patterns, error handling, goroutine management
- Python: PEP 8, type hints, anti-patterns
- JavaScript/TypeScript: Modern patterns, React hooks, type safety

## Output Formats

### Console Text Report

```
📊 Code Review Report
════════════════════════════════════

🔒 Security Issues (2)
  ❌ CRITICAL: SQL injection risk in user.go:45
  ⚠️  WARNING: Hardcoded API key in config.py:12

✅ Code Quality (3 issues)
  💡 Consider using context.Context in handler.go:89

📚 Documentation
  ✅ README.md updated
  ⚠️  Missing docstrings in utils.py

✨ Summary
  - 2 critical issues found
  - 1 warning
  - 3 improvements suggested
  - Documentation: Incomplete

Next Steps:
1. Fix SQL injection vulnerability
2. Remove hardcoded credentials
3. Update Python documentation
```

### GitHub-Style Comments

```markdown
## Review Comments (Copy-paste to GitHub)

### Security Issues
- [ ] 🔒 Fix SQL injection in `user.go:45`
  ```suggestion
  query := "SELECT * FROM users WHERE id = ?"
  db.Query(query, userID)
  ```

- [ ] ⚠️ Remove hardcoded credentials in `config.py:12`

### Code Quality
- [ ] 💡 Consider using `context.Context` in `handler.go:89`
```

## Examples

### Example 1: Review Local Changes

```bash
# Make some code changes
vim src/handler.go

# Review the changes
/review
```

**Output:**
```
📊 Code Review Report
════════════════════════════════════

Files Reviewed: 1 file (handler.go)
Lines Changed: +45, -12

🔒 Security Issues (1)
  ⚠️ WARNING: No authentication check in handler.go:23
     Recommendation: Add authentication middleware

✅ Code Quality (2 issues)
  💡 Consider adding error context in handler.go:45
  💡 Extract duplicate validation logic in handler.go:78

📚 Documentation
  ⚠️ Missing godoc comment for exported function HandleRequest

✨ Summary
  - 1 security warning
  - 2 improvements suggested
  - Add authentication and improve error handling
```

### Example 2: Review GitHub PR

```bash
/review https://github.com/myorg/myrepo/pull/456
```

**Output:**
```
🔍 Pull Request Review
════════════════════════════════════

PR #456: Add user authentication
Repository: myorg/myrepo
Author: @developer
Files Changed: 5 files (+234, -45 lines)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚨 CRITICAL ISSUES (1)
❌ Password stored in plain text (auth.py:67)
   Fix: Use bcrypt for password hashing

🔒 SECURITY CONCERNS (2)
⚠️ Session token not cryptographically random (session.py:23)
⚠️ Missing rate limiting on login endpoint (routes.py:45)

✅ CI/CD STATUS
✅ Tests passing
✅ Linting passed
⚠️ Code coverage decreased by 2%

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 RECOMMENDATION: Request Changes

Critical security issue must be fixed before merge.
```

### Example 3: Security-Only Review

```bash
/review --security-only
```

**Output:**
```
🔒 Security Analysis Report
════════════════════════════════════

Scope: 8 files analyzed
Risk Level: High

🚨 CRITICAL VULNERABILITIES (1)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. SQL Injection - CVSS 9.8
   Location: user_service.py:45

   Risk:
   Attacker can execute arbitrary SQL queries, potentially
   accessing or deleting all database data.

   Evidence:
   ```python
   query = f"SELECT * FROM users WHERE id = {user_id}"
   ```

   Fix:
   ```python
   query = "SELECT * FROM users WHERE id = ?"
   cursor.execute(query, (user_id,))
   ```

🎯 IMMEDIATE ACTION REQUIRED
1. Fix SQL injection in user_service.py:45
2. Scan for similar patterns in codebase
```

## Configuration

### Plugin Settings

Edit `.claude-plugin/plugin.json` to customize plugin metadata:

```json
{
  "name": "code-review-plugin",
  "description": "Your custom description",
  "version": "1.0.0",
  "author": {
    "name": "Your Name"
  }
}
```

### Review Levels

Customize the detail level in command prompts:

- **Critical Only**: Bugs + Security vulnerabilities only
- **Standard** (default): Bugs + Security + Documentation
- **Detailed**: All above + Style + Refactoring suggestions

## Supported Languages

### Fully Supported

| Language | Version Support | Features |
|----------|----------------|----------|
| Go | 1.16+ | Idiomatic patterns, error handling, concurrency |
| Python | 3.8+ | PEP 8, type hints, security patterns |
| JavaScript | ES6+ | Modern syntax, async/await, React patterns |
| TypeScript | 4.0+ | Type safety, strict mode, generics |

### Detection Capabilities

The plugin automatically detects language from:
- File extensions (`.go`, `.py`, `.js`, `.ts`, `.jsx`, `.tsx`)
- Shebang lines (`#!/usr/bin/env python3`)
- Package files (`go.mod`, `package.json`, `requirements.txt`)

## Troubleshooting

### `gh` CLI Not Found

```bash
# Install GitHub CLI
brew install gh  # macOS
# or download from https://cli.github.com/

# Authenticate
gh auth login
```

### No Changes Detected

```bash
# Check git status
git status

# If no changes, review last commit
git diff HEAD~1..HEAD
```

### Permission Denied for Private Repos

```bash
# Ensure gh CLI is authenticated
gh auth status

# Re-authenticate if needed
gh auth login
```

## Development

### Plugin Structure

```
code-review-plugin/
├── .claude-plugin/
│   └── plugin.json              # Plugin metadata
├── commands/
│   └── review.md                # /review command
├── agents/
│   ├── code-reviewer.md         # Main review agent
│   ├── security-expert.md       # Security specialist
│   └── performance-analyzer.md  # Performance specialist
├── skills/
│   ├── pr-reviewer/
│   │   ├── SKILL.md            # PR review skill
│   │   └── github-patterns.json
│   └── code-quality/
│       ├── SKILL.md            # Code quality skill
│       └── language-rules.json
└── README.md                    # This file
```

### Adding Language Support

To add support for a new language:

1. Add language rules to `skills/code-quality/language-rules.json`
2. Update language detection in command and skill prompts
3. Add language-specific security patterns
4. Update documentation

### Extending Security Checks

Add new security patterns to:
- `agents/security-expert.md`: Add detection logic
- `skills/pr-reviewer/github-patterns.json`: Add pattern definitions

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

MIT License - See LICENSE file for details

## Support

For issues, questions, or feature requests:
- Open an issue in the repository
- Contact the maintainer: katalabut

## Changelog

### v1.0.0 (2025-01-07)

- Initial release
- Support for Go, Python, JavaScript/TypeScript
- Command-based review with `/review`
- Three specialized agents (code-reviewer, security-expert, performance-analyzer)
- Two skills (pr-reviewer, code-quality)
- GitHub PR integration via `gh` CLI
- OWASP Top 10 security checks
- Console and GitHub-style output formats

---

**Built for Claude Code** - Enhancing code quality through AI-powered reviews
