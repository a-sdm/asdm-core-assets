# ASDM Action: PR Summary

## Overview

This action generates a comprehensive summary of Pull Request
changes by calling GitHub MCP tools, providing a high-level
overview of modifications and their impact.

## Purpose

- Provide quick overview of PR changes
- Summarize affected components and modules
- Identify key modifications and their purposes
- Help reviewers understand PR scope before detailed review

## Prerequisites

- GitHub MCP Server must be connected and available
- PR number is required

## Input

- **PR Number**: The identifier of the Pull Request (required)
- **Detail Level**: Optional (brief/standard/detailed),
  default: standard
- **Focus Areas**: Optional areas to highlight
  (e.g., API, database, UI)

## Steps

### 0. Resolve Repository

Determine the GitHub `owner` and `repo` from the current workspace:

1. Run `git remote -v` to find the remote URL
2. Parse `owner` and `repo` from the URL
   - HTTPS: `https://github.com/{owner}/{repo}.git`
   - SSH: `git@github.com:{owner}/{repo}.git`

### 1. Get PR Metadata

**MCP Call**: `get_pull_request`

| Parameter | Value |
|-----------|-------|
| `owner` | Resolved from git remote |
| `repo` | Resolved from git remote |
| `pull_number` | `{PR_NUMBER}` |

This returns:
- PR title and description
- Source and target branches
- Author information
- Status (open, merged, closed)
- Number of changed files, additions, deletions
- Labels, milestone, linked issues

### 2. Get Changed Files

**MCP Call**: `get_pull_request_files`

| Parameter | Value |
|-----------|-------|
| `owner` | Resolved from git remote |
| `repo` | Resolved from git remote |
| `pull_number` | `{PR_NUMBER}` |

This returns the full list of changed files with per-file
additions, deletions, status, and patch content.

### 3. Analyze Changes

Categorize and analyze the changes based on MCP responses:

#### By File Type
- Source code files (by language)
- Configuration files
- Documentation files
- Test files
- Asset files

#### By Component/Module
- Group files by logical component
- Identify affected modules
- Map changes to architectural layers

#### By Change Type
- New features (detect from PR title prefix `feat:`)
- Bug fixes (detect from PR title prefix `fix:`)
- Refactoring (detect from PR title prefix `refactor:`)
- Documentation updates (detect from PR title prefix `docs:`)
- Configuration changes
- Test additions/modifications (detect from `fix:` prefix)

### 4. Assess Risk

Based on the analysis:

- File count and line change volume
- Core module vs peripheral module changes
- Security-sensitive file modifications
- Database or API schema changes

### 5. Generate Summary

Create a structured summary using the format below.

## Output Format

### Brief Summary

```
PR #{PR_NUMBER}: {PR_TITLE}

Statistics:
- Files changed: {count}
- Additions: +{additions}
- Deletions: -{deletions}

Summary:
{Brief description of changes}

Type: feature|fix|refactor|docs|test
Risk: low|medium|high
```

### Standard Summary

```
PR #{PR_NUMBER}: {PR_TITLE}

Author: {author}
Branch: {source_branch} -> {target_branch}

## Statistics
- Files changed: {count}
- Additions: +{additions}
- Deletions: -{deletions}

## Change Summary
{Detailed summary of what was changed and why}

## Affected Components
- **{Component Name}**: {change description}
- **{Component Name}**: {change description}

## Change Categories
- Features: {count}
- Fixes: {count}
- Refactoring: {count}
- Documentation: {count}
- Tests: {count}

## Risk Assessment
Level: {low/medium/high}
Reasons: {reasons}

## Testing Recommendations
- {recommendation 1}
- {recommendation 2}
```

### Detailed Summary

Includes all of standard summary plus:

```
## Changed Files

### Added Files
- `path/to/file1.ext` - {description}
- `path/to/file2.ext` - {description}

### Modified Files
- `path/to/file3.ext` - {description of changes}

### Deleted Files
- `path/to/file4.ext` - {reason for deletion}

## Dependencies
- New dependencies added: {list}
- Dependencies removed: {list}
- Dependencies updated: {list}

## Key Code Changes
### {Section Name}
{Highlighted important code changes with context}
```

## Usage Examples

### Basic Usage

```shell
# Using slash command
/asdm-pr-summary 123

# Or follow the instruction
Follow the instructions in
.asdm/toolsets/code-review/actions/asdm-pr-summary.md
```

### Brief Summary

```shell
# Get brief summary
/asdm-pr-summary 123 --brief
```

### Detailed Summary

```shell
# Get detailed summary
/asdm-pr-summary 123 --detailed
```

## Integration with Other Actions

This action complements:

- `asdm-pr-diff`: Uses diff data for detailed analysis
- `asdm-pr-review`: Provides context for review decisions

## MCP Tool Reference

| MCP Server | Tool | Purpose |
|------------|------|---------|
| GitHub MCP Server | `get_pull_request` | Get PR metadata |
| GitHub MCP Server | `get_pull_request_files` | Get changed files with patch |
| GitHub MCP Server | `get_file_contents` | Get file content by path |

## Notes

- Summary quality depends on PR description quality
- Consider generating summaries after major commits
- Use in combination with diff for comprehensive understanding
- Update summary if PR changes significantly

## Configuration

Refer to [pr-analysis-spec.md](../specs/pr-analysis-spec.md)
for detailed analysis methodology.
