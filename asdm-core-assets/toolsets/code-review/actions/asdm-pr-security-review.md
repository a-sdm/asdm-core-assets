# ASDM Action: PR Security Review

## Overview

This action performs a security-focused code review on a Pull
Request by fetching data via GitHub MCP tools, specifically
targeting security vulnerabilities, authentication issues, and
data protection concerns.

## Purpose

- Identify security vulnerabilities and risks
- Verify authentication and authorization implementation
- Check for common attack vectors
- Ensure secure data handling practices
- Provide security-specific remediation guidance

## Prerequisites

- GitHub MCP Server must be connected and available
- PR number or PR URL is required

## Input

- **PR Number**: The identifier of the Pull Request (required)
- **Focus Areas**: Optional specific security concerns to focus on

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

**IMPORTANT**: The language detection is the FIRST step before
any review content generation.

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

### 3. Gather Security Context

Using the PR metadata and changed files:

- Understand the security requirements of changed components
- Identify sensitive data flows from file paths and patch content
- Map authentication and authorization boundaries
- For security-sensitive files, use `get_file_contents` to read
  full context when patch is insufficient

### 4. Input Validation Analysis

Based on the patch content from `get_pull_request_files`:

- Check all user inputs are validated
- Verify sanitization of untrusted data
- Identify potential injection points

### 5. Authentication & Authorization Review

- Verify authentication mechanisms
- Check authorization logic
- Review session management
- Assess privilege escalation risks

### 6. Data Protection Analysis

- Review sensitive data handling
- Check encryption implementations
- Verify secure data storage
- Assess data exposure risks

### 7. Common Vulnerability Scan

- SQL Injection checks
- XSS vulnerability detection
- CSRF token validation
- Command injection analysis
- Path traversal detection

### 8. Generate Security Report

- Document all security findings
- Provide severity classification
- Suggest remediation steps
- Reference security best practices

## Output

- Security-focused review report with:
  - Vulnerability summary
  - Detailed findings with CVSS-like severity
  - Remediation recommendations
  - Code examples for fixes
  - Security best practices references

## Security Checklist

### Authentication
- [ ] Password handling is secure (hashing, no plaintext)
- [ ] Session tokens are properly generated and validated
- [ ] Multi-factor authentication implemented correctly
- [ ] Account lockout mechanism exists
- [ ] Password reset flow is secure

### Authorization
- [ ] Role-based access control implemented
- [ ] Permission checks are consistent
- [ ] No authorization bypass vulnerabilities
- [ ] Resource ownership verified
- [ ] Principle of least privilege followed

### Input Validation
- [ ] All user inputs are validated
- [ ] Input sanitization applied
- [ ] Type checking enforced
- [ ] Length limits applied
- [ ] Special characters handled

### Injection Prevention
- [ ] Parameterized queries used for database access
- [ ] Output encoding applied for XSS prevention
- [ ] Command injection prevented
- [ ] LDAP injection prevented
- [ ] No eval() or similar dangerous functions

### Data Protection
- [ ] Sensitive data encrypted at rest
- [ ] Sensitive data encrypted in transit
- [ ] No sensitive data in logs
- [ ] No sensitive data in URLs
- [ ] Proper data retention policies

### API Security
- [ ] API authentication required
- [ ] Rate limiting implemented
- [ ] Input validation on all endpoints
- [ ] Proper error handling without info leakage
- [ ] CORS configured correctly

## Severity Classification

| Severity | CVSS Range | Description |
|----------|------------|-------------|
| Critical | 9.0-10.0 | Exploitable without user interaction, full system compromise |
| High | 7.0-8.9 | Significant vulnerability, potential data breach |
| Medium | 4.0-6.9 | Moderate risk, requires specific conditions |
| Low | 0.1-3.9 | Minor security issue, limited impact |
| Info | N/A | Security best practice recommendation |

## MCP Tool Reference

| MCP Server | Tool | Purpose |
|------------|------|---------|
| GitHub MCP Server | `get_pull_request` | Get PR metadata |
| GitHub MCP Server | `get_pull_request_files` | Get changed files with patch |
| GitHub MCP Server | `get_file_contents` | Get full file content for context |

## Output Format

```markdown
# Security Review Report

## Executive Summary
[Brief overview of security posture]

## Critical Vulnerabilities
[Immediate security risks requiring urgent fix]

## High Severity Issues
[Significant security concerns]

## Medium Severity Issues
[Moderate security risks]

## Low Severity Issues
[Minor security improvements]

## Security Recommendations
[Best practice suggestions]

## Remediation Summary
[Quick reference for all required fixes]
```

## Notes

- Focus on actionable security feedback
- Provide specific remediation code examples
- Reference OWASP Top 10 and CWE databases
- Consider threat modeling context
- Balance security with usability
