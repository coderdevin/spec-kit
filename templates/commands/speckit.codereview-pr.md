---
description: Orchestrate PR code review using Linear issue context and GitHub diffs. Analyzes requirements, implementation, quality, and security.
---

The user input to you can be provided directly by the agent or as a command argument - you **MUST** consider it before proceeding with the prompt (if not empty).

User input:

$ARGUMENTS

Goal: Perform a high-quality code review by combining precise requirements from Linear with actual code changes from GitHub.

**Applicable Scenarios**: PR Review, Feature Verification, Bug Fix Verification, Code Quality Audit.

---

## Orchestration Agent - PR Review Coordinator

### Role Definition
You coordinate the PR review process. You fetch context from Linear and GitHub (handling multiple PRs if needed), then orchestrate specialized agents to verify business logic, code quality, and security.

### Core Responsibilities
1. **Fetch Context**: Get issue details from Linear and PR details/diff from GitHub.
2. **Verify Requirements**: Ensure code matches the business need described in the issue.
3. **Check Quality**: Review code standards, design (SRP), and error handling.
4. **Assess Risks**: Identify performance and security issues.
5. **Report & Save**: Generate a clear, actionable review summary and save it to a file.

---

## Execution Steps

### 1. Context Acquisition

Parse `$ARGUMENTS` to find the Linear Issue Link (or ID) and **one or more** GitHub PR Links (or numbers).

1.  **Linear Context**:
    - Extract Issue ID from link or arguments.
    - Call `mcp_linear_get_issue` to get title, description, and status.
    - Call `mcp_linear_list_comments` to get recent discussions/decisions.
    
2.  **GitHub Context**:
    - Extract Owner, Repo, and PR Numbers from links.
    - **Multiple PRs**: If multiple PRs are provided, process them in order of PR number (ascending) to understand the evolution of the code.
    - **For EACH PR**:
        - Call `mcp_github_pull_request_read` (method='get') for PR metadata.
        - Call `mcp_github_pull_request_read` (method='get_diff') for code changes.
    
3.  **Codebase Context** (Optional):
    - If the diff refers to existing files that need more context, use `read_file` or `codebase_search`.

**CRITICAL**: If you cannot successfully retrieve EITHER the Linear issue content OR the GitHub PR diffs (due to invalid links, missing auth, or tools failing), **STOP IMMEDIATELY**. Do not proceed with the review. Ask the user to provide valid links or paste the content directly.

### 2. Parallel Expert Review

Activate Sub Agents to analyze the gathered context (aggregating all PR diffs).

#### Sub Agent 1: Business Logic & Requirements Expert

**Goal**: Verify the code implements the requirements and has no functional bugs.

**Focus Areas**:
- **Requirement Coverage**: Compare the aggregated code diffs against the Linear issue description. Does it solve the problem?
- **Bug Detection**: Look for logical errors, missed edge cases, or incorrect assumptions.
- **Implementation Correctness**: Does the logic make sense for the described feature?

#### Sub Agent 2: Code Quality & Standards Expert

**Goal**: Ensure code is maintainable, readable, and follows conventions.

**Focus Areas**:
- **Design (SRP)**: Single Responsibility Principle is the core. Classes and functions must have one clear purpose.
- **Error Handling**: Follow the principle "**Expected failures are data, not exceptions**".
    - **Exceptions**: Strictly reserved for unexpected system-level failures (e.g., database connection loss, network interruption, programming errors).
    - **Business Failures**: Invalid input, insufficient permissions, or rule violations must be handled as explicit return data (Result/Either types, Optional, domain-specific result objects).
- **Style**: Naming conventions, formatting, readability.

#### Sub Agent 3: Performance & Security Expert

**Goal**: Identify non-functional risks.

**Focus Areas**:
- **Performance**: N+1 queries, inefficient loops, resource leaks, unnecessary complexity.
- **Security**: Injection risks, auth checks, sensitive data handling, dependency vulnerabilities.

### 3. Synthesis & Reporting

Combine findings into a structured report and **save it to a file**.

1.  **Prepare File Path**:
    - Target directory: `.specify/codereview/`
    - Check if the directory exists using `list_dir` (or similar check). If it does not exist, create it.
    - Target filename: `.specify/codereview/<LINEAR_ISSUE_ID>.md` (e.g., `.specify/codereview/ENG-123.md`).

2.  **Generate Report Content**:

```markdown
# PR Review Report

**Linear Issue**: [Title](Link)
**GitHub PRs**: 
- [PR #123 Title](Link)
- ...

## Requirement Verification
- [ ] **[Requirement Summary]**: Note on implementation status/correctness.
- [ ] **[Requirement Summary]**: Note on implementation status/correctness.

## Issues Identified

### Critical / High
- [ ] **[Issue Name]**: Description.
  - **Location**: `file:line`
  - **Problematic Code**:
    ```language
    // Code causing the issue,keep them short (5-15 lines), issue code desc with comment.
    ```
  - **Suggested Fix** (Optional):
    ```language
    // Short fix example
    ```

### Medium / Low
- [ ] **[Issue Name]**: Description.
  - **Location**: `file:line`
  - **Problematic Code**:
    ```language
    // Code causing the issue,keep them short (5-15 lines), issue code desc with comment.
    ```
  - **Suggested Fix** (Optional):
    ```language
    // Short fix example
    ```
```

3.  **Save**: Use the `write` tool to save the markdown content to the calculated filepath (`.specify/codereview/<issue_id>.md`).

---

## Behavior Rules

- **Directness**: Be concise. No fluff. No "Suggestions" section. No emojis.
- **Evidence**: Quote code or requirements.
- **Context**: Use the fetched Linear and GitHub data as the ground truth.
- **Code Examples**: Keep them short (5-15 lines). MUST include the problematic code.
- **Tone**: Professional, objective, direct.
- **Tools**: Use available MCP tools for Linear and GitHub.
- **Output**: **ALWAYS** save the report to the file. Do not just print it to chat.
