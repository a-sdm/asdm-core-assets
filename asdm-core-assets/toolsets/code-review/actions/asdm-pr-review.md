# ASDM Action: PR Code Review

## Overview

This action performs a comprehensive code review on a Pull Request
by fetching data via GitHub MCP tools and analyzing code quality,
security, performance, and best practices.

## Purpose

- Identify code quality issues and potential bugs
- Detect security vulnerabilities
- Check adherence to coding standards and best practices
- Provide actionable improvement suggestions
- Generate structured review report

## Prerequisites

- GitHub MCP Server must be connected and available
- PR number or PR URL is required

## Input

- **PR Number**: The identifier of the Pull Request (required)
- **Focus Areas**: Optional specific areas to focus on
  (e.g., security, performance, style)

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

**IMPORTANT**: All output must use the detected language throughout
the entire process.

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

This provides PR metadata, description, and the full file
change list with patch content.

### 3. Analyze Code Changes

Based on the patch content from `get_pull_request_files`:

- Review code structure and organization
- Check for code smells and anti-patterns
- Verify logic correctness and edge cases
- Assess test coverage
- For files with large changes, optionally use `get_file_contents`
  to read the full file context

### 4. Security Analysis

- Check for common security vulnerabilities
- Review authentication and authorization logic
- Identify potential injection risks
- Verify sensitive data handling

### 5. Performance Review

- Identify potential performance bottlenecks
- Check for inefficient algorithms
- Review database query patterns
- Assess memory usage patterns

### 6. Best Practices Check

- Verify coding style consistency
- Check naming conventions
- Review error handling patterns
- Assess code maintainability

### 7. Generate Review Report

- Summarize findings by severity
- Provide specific line-level feedback
- Suggest improvements with code examples
- Assign severity levels to each finding

## Output

- Structured review report with:
  - Executive summary
  - Detailed findings by category
  - Severity levels (Critical/High/Medium/Low/Info)
  - Line-level comments with suggestions
  - Overall recommendation

## Review Dimensions

### 1. Code Quality
- Readability and maintainability
- Code duplication
- Complexity analysis
- Dead code detection

### 2. Security
- Injection vulnerabilities (SQL, XSS, Command)
- Authentication and authorization issues
- Sensitive data exposure
- Input validation

### 3. Performance
- Algorithm efficiency
- Resource management
- Caching opportunities
- Database query optimization

### 4. Best Practices
- Design patterns usage
- Error handling
- Logging and monitoring
- Documentation completeness

### 5. Testing
- Test coverage
- Test quality
- Edge case coverage
- Mock usage appropriateness

## Severity Definitions

| Severity | Description | Action Required |
|----------|-------------|-----------------|
| Critical | Security vulnerabilities, breaking bugs, data loss risks | Must fix before merge |
| High | Significant issues that could cause problems | Should fix before merge |
| Medium | Code quality issues, maintainability concerns | Recommend fix |
| Low | Minor improvements, style preferences | Optional fix |
| Info | Informational notes, suggestions | No action required |

## MCP Tool Reference

| MCP Server | Tool | Purpose |
|------------|------|---------|
| GitHub MCP Server | `get_pull_request` | Get PR metadata |
| GitHub MCP Server | `get_pull_request_files` | Get changed files with patch |
| GitHub MCP Server | `get_file_contents` | Get full file content |

## Review Process Flow

```
+------------------+
| Resolve Repo     |
+--------+---------+
         |
         v
+------------------+
| Language Detect  |
+--------+---------+
         |
         v
+------------------+
| MCP: PR + Files  |
+--------+---------+
         |
         v
+------------------+
| Analyze Diff     |
+--------+---------+
         |
         v
+------------------+     +------------------+
| Security Check   |---->| Quality Check    |
+--------+---------+     +--------+---------+
         |                        |
         v                        v
+------------------+     +------------------+
| Perf Check       |---->| Best Practices   |
+--------+---------+     +--------+---------+
         |                        |
         +------------+-----------+
                      |
                      v
          +------------------+
          | Generate Report  |
          +------------------+
```

## Output Format

The review report follows this structure:

```markdown
# Code Review Report

## Summary
[Brief overview of the PR and overall assessment]

## Critical Issues
[List of critical issues that must be fixed]

## High Priority Issues
[List of high priority issues]

## Medium Priority Issues
[List of medium priority issues]

## Low Priority Issues
[List of low priority issues]

## Suggestions
[List of informational suggestions]

## Overall Recommendation
[Approve / Request Changes / Need Discussion]
```

## Notes

- Focus on constructive feedback with actionable suggestions
- Prioritize issues by impact and likelihood
- Provide code examples for suggested improvements
- Consider the project's specific context and constraints
