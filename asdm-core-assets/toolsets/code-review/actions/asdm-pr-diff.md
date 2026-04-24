# ASDM Action: PR Diff

## Overview

This action retrieves and formats the diff of a Pull Request by
calling GitHub MCP tools, providing a structured view of all changes
for analysis.

## Purpose

- Retrieve PR metadata and changed files via GitHub MCP Server
- Format diff output for easy analysis
- Support code review workflows
- Enable change analysis

## Prerequisites

- GitHub MCP Server must be connected and available
- PR number is required
- Appropriate repository access permissions

## Input

- **PR Number**: The identifier of the Pull Request (required)
- **File Filter**: Optional file path pattern to filter changes
- **Context Lines**: Number of context lines to show (default: 3)

## Steps

### 0. Resolve Repository

Determine the GitHub `owner` and `repo` from the current workspace:

1. Run `git remote -v` to find the remote URL
2. Parse `owner` and `repo` from the URL
   - HTTPS: `https://github.com/{owner}/{repo}.git`
   - SSH: `git@github.com:{owner}/{repo}.git`

### 1. Get PR Basic Information

Use the GitHub MCP Server to retrieve PR metadata:

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

### 2. List Changed Files

**MCP Call**: `get_pull_request_files`

| Parameter | Value |
|-----------|-------|
| `owner` | Resolved from git remote |
| `repo` | Resolved from git remote |
| `pull_number` | `{PR_NUMBER}` |

This returns the full list of changed files with per-file
additions, deletions, and patch content.

### 3. (Optional) Fetch File Content for Specific Files

For files that need deeper analysis, use:

**MCP Call**: `get_file_contents`

| Parameter | Value |
|-----------|-------|
| `owner` | Resolved from git remote |
| `repo` | Resolved from git remote |
| `path` | `{file_path}` |
| `branch` | Target branch of the PR |

### 4. Parse and Format Output

Organize the results into the structured format below.

## Output Format

### Summary View

```
PR #{PR_NUMBER}: {PR_TITLE}
Author: {author}
Branch: {source_branch} -> {target_branch}
Files: {files_changed} changed, +{additions} -{deletions}

Changed Files:
1. src/path/to/file1.ext (+20, -5) [modified]
2. src/path/to/file2.ext (+100, -0) [added]
3. src/path/to/file3.ext (+0, -10) [deleted]
```

### Detailed Diff View

For each file, extract the patch content from
`get_pull_request_files` response and present:

```diff
--- a/src/path/to/file.ext
+++ b/src/path/to/file.ext
@@ -10,6 +10,8 @@ context line
-removed line
+added line
+another added line
 context line
```

## File Status Types

| Status | Description |
|--------|-------------|
| added | New file created |
| modified | Existing file changed |
| deleted | File removed |
| renamed | File renamed/moved |

## Usage Examples

### Basic Usage

```shell
# Using slash command
/asdm-pr-diff 123

# Or follow the instruction
Follow the instructions in
.asdm/toolsets/code-review/actions/asdm-pr-diff.md
```

### Filter by File Pattern

```shell
# Get diff for specific files
/asdm-pr-diff 123 --filter "src/components/*.tsx"
```

### Get More Context

```shell
# Show more context lines
/asdm-pr-diff 123 --context 10
```

## Integration with Other Actions

This action is typically used as a prerequisite for:

- `asdm-pr-review`: Provides the diff for review analysis
- `asdm-pr-summary`: Uses diff data for summary generation

## MCP Tool Reference

| MCP Server | Tool | Purpose |
|------------|------|---------|
| GitHub MCP Server | `get_pull_request` | Get PR metadata |
| GitHub MCP Server | `get_pull_request_files` | Get changed files with patch |
| GitHub MCP Server | `get_file_contents` | Get file content by path |

## Notes

- Large PRs may have truncated patch content; use `get_file_contents`
  for specific files when needed
- Binary files show only metadata, not content diff
- Consider file size limits for very large diffs
- Use file filtering for focused analysis

## Configuration

Refer to [pr-analysis-spec.md](../specs/pr-analysis-spec.md)
for detailed analysis methodology.
