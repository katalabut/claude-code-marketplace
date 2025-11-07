---
name: prd-analyzer
description: Automatically analyzes PRD documents, extracts requirements, validates completeness, and suggests task structures. Use when user mentions PRD, product requirements, or references PRD files.
version: 1.0.0
allowed-tools: Read, Write
---

# PRD Analyzer Skill

Specialized skill for analyzing Product Requirements Documents and extracting actionable tasks.

## Capabilities
- Parse PRD markdown files
- Extract features and requirements  
- Identify missing sections
- Validate completeness against template
- Suggest task breakdowns
- Generate task hierarchy

## When to Activate
- User mentions "PRD" or "product requirements"
- User provides path to .md file in docs/prd/
- User asks to analyze requirements document
- User wants to extract tasks from document

## Analysis Process

### 1. Read PRD File
Parse markdown structure and extract:
- Document metadata
- Features and requirements
- Task breakdowns
- Acceptance criteria
- Dependencies
- Priorities

### 2. Validate Completeness
Check against standard sections:
- ✓ Executive Summary
- ✓ Problem Statement
- ✓ Goals & Success Metrics
- ✓ User Personas
- ✓ Features & Requirements
- ✓ Technical Architecture
- ✓ Timeline

Flag missing or incomplete sections.

### 3. Extract Task Structure
Identify hierarchical breakdown:
- Themes
- Epics
- Features
- Tasks
- Subtasks

Parse task attributes:
- IDs (decimal notation)
- Priorities (P0-P3)
- Estimates
- Dependencies
- Acceptance criteria

### 4. Suggest Improvements
Recommend:
- Missing sections to add
- Vague requirements to clarify
- Tasks needing more detail
- Dependencies to document
- Metrics to define

## Output Format

```
📊 PRD Analysis: feature-name-prd.md

✅ Complete Sections (8/12):
   ✓ Executive Summary
   ✓ Problem Statement
   [...]

⚠️  Incomplete Sections (3/12):
   ⚠ User Personas - needs details
   [...]

❌ Missing Sections (1/12):
   ✗ Go-to-Market Strategy

📋 Task Structure Found:
   2 Themes
   5 Epics
   12 Features
   45 Tasks

💡 Recommendations:
   1. Add specific KPI targets
   2. Define secondary personas
   3. Document launch strategy
   
Quality Score: 75/100
```

## Integration

Works seamlessly with:
- `/prd analyze` command
- `/prd parse` command
- Taskmaster MCP `parse_prd` tool

Automatically activates when PRD mentioned in conversation.
