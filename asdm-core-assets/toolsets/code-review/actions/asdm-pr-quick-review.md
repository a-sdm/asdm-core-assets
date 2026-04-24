# ASDM Action: PR Quick Review

## Overview

This action performs a quick, lightweight code review on a Pull
Request by fetching data via GitHub MCP tools, focusing on common
issues and providing rapid feedback.

## Purpose

- Provide fast feedback on common code issues
- Check basic code quality and style
- Identify obvious bugs and errors
- Verify basic best practices
- Enable quick iteration cycles

## Prerequisites

- GitHub MCP Server must be connected and available
- PR number or PR URL is required

## Input

- **PR Number**: The identifier of the Pull Request (required)
- **Focus Areas**: Optional specific focus areas

## Steps

### 0. Resolve Repository

Determine the GitHub `owner` and `repo` from the current workspace:

1. Run `git remote -v` to find the remote URL
2. Parse `owner` and `repo` from the URL
   - HTTPS: `https://github.com/{owner}/{repo}.git`
   - SSH: `git@github.com:{owner}/{repo}.git`

### 1. Language Detection (MUST be first)

Detect and use the current environment's response language:

1. Check system/user language settings or environment configuration
2. Identify the primary language used in project documentation
3. Apply the detected language consistently to ALL output

### 2. Gather PR Data via MCP

Call the following MCP tools in parallel:

**MCP Call**: `get_pull_request`

| Parameter | Value |
|-----------|-------|
| `owner` | Resolved from git remote |
| `repo` | Resolved from git remote |
| `pull_number` | `{PR_NUMBER}` |

**MCP Call**: `get_pull_request_files`

| Parameter | Value |
|-----------|-------|
| `owner` | Resolved from git remote |
| `repo` | Resolved from git remote |
| `pull_number` | `{PR_NUMBER}` |

### 3. Quick Scan

Based on the MCP response:

- Identify file types and changes from `get_pull_request_files`
- Assess scope of changes (file count, line count)
- Categorize changes (source code, config, docs, tests)

### 4. Basic Quality Check

Using the patch content from `get_pull_request_files`:

- Check for syntax errors
- Verify code formatting
- Identify obvious logic errors
- Check for missing error handling

### 5. Common Issues Detection

- Detect common anti-patterns
- Check for TODO/FIXME comments
- Identify unused imports/variables
- Check for debug code left in

### 6. Style Verification

- Check naming conventions
- Verify consistent formatting
- Check line length limits
- Verify proper indentation

### 7. Quick Summary

- Summarize findings
- Highlight quick wins
- Provide actionable feedback

## Output

- Concise review summary with:
  - Quick assessment
  - Top issues to address
  - Style/formatting suggestions
  - Quick wins for improvement

## Quick Check Items

### Code Quality Quick Checks
- [ ] No obvious syntax errors
- [ ] No hardcoded values that should be configurable
- [ ] No commented-out code
- [ ] No console.log/print statements in production code
- [ ] Proper error handling present

### Style Quick Checks
- [ ] Consistent naming conventions
- [ ] Proper indentation
- [ ] Reasonable line lengths
- [ ] No trailing whitespace
- [ ] Proper file encoding

### Common Anti-Patterns
- [ ] No deep nesting (more than 3-4 levels)
- [ ] No god functions (too long/too many responsibilities)
- [ ] No magic numbers without explanation
- [ ] No duplicate code blocks
- [ ] No unused variables or imports

### Documentation Quick Checks
- [ ] Public functions have comments
- [ ] Complex logic is explained
- [ ] No TODO without ticket reference
- [ ] No FIXME without explanation

## Quick Severity Guide

| Level | Description | Action |
|-------|-------------|--------|
| Must Fix | Errors or critical issues | Fix before merge |
| Should Fix | Style or quality issues | Recommend fixing |
| Consider | Minor improvements | Optional |

## MCP Tool Reference

| MCP Server | Tool | Purpose |
|------------|------|---------|
| GitHub MCP Server | `get_pull_request` | Get PR metadata |
| GitHub MCP Server | `get_pull_request_files` | Get changed files with patch |

## Output Format

```markdown
# Quick Review Summary

## Overview
[1-2 sentence summary of the changes]

## Must Fix
[Critical issues that should be addressed]

## Should Fix
[Recommended improvements]

## Consider
[Nice-to-have suggestions]

## Quick Stats
- Files changed: X
- Lines added: Y
- Lines removed: Z
- Estimated review complexity: Low/Medium/High

## Verdict
[Ready for detailed review / Needs revision first]
```

## Notes

- Focus on speed and actionable feedback
- Not a replacement for comprehensive review
- Best used for early feedback in PR lifecycle
- Can be followed by detailed review if needed
