---
description: Validate checked issues from code review reports, discover similar patterns across the codebase, and generate a validation report with pattern analysis.
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

1. **Locate Report**: Find code review report in `.specify/reviews/` (use user-specified file or most recent). Detect programming languages from file extensions.

2. **Extract Checked Issues**: Parse markdown task lists for `[x]` items. Extract:
   - Issue title, priority, code location, category
   - Ignore unchecked `[ ]` items

3. **Verify Issues**: For each checked issue:
   - Read target code with `read_file`
   - Confirm issue exists in current codebase
   - Assess validity (✅ Valid / ⚠️ Partially Valid / ❌ Invalid / 🔄 Modified)
   - Document validation reason

4. **Discover Patterns**: For each validated issue, search codebase for similar instances:
   - Use `grep` for exact patterns (e.g., `: any`, `except:`, raw types)
   - Use `codebase_search` for semantic patterns (e.g., "large functions", "duplicated code")
   - Record code location, line numbers, and problematic code for each pattern
   - Prioritize patterns appearing 3+ times

5. **Generate Report and Save**: Write to `.specify/reviews/codereview-check-{name}-{timestamp}.md` with:
   - Header: source review, statistics, languages detected
   - Each issue with: validation status, validation reason, similar patterns found

## Report Structure

```markdown
# {Component/Module Name} - Code Review Validation Report

| Item | Details |
|------|---------|
| **Generated** | {timestamp} |
| **Source Review** | `.specify/reviews/{original-filename}` |
| **Languages** | {detected languages} |
| **Total Issues** | {count} |

---

## 🔍 Validation Results

### Issue 1️⃣ {Issue Title}

**Status**: {✅ Valid / ⚠️ Partially Valid / ❌ Invalid / 🔄 Modified} · 
**Validation Reason**: {detailed explanation of why this issue is valid/invalid/modified}

**Similar Pattern Search**:

**Similar Issue 1: {Issue Title}**
**Location**: `{filepath}`  
**Lines**: {line-start}-{line-end}

```language
{problematic code snippet}
```

**Description**: {问题描述}

** Similar Issue 2: {Issue Title} **

**Location**: `{filepath}`  
**Lines**: {line-start}-{line-end}

```language
{problematic code snippet}
```

**Description**: {问题描述}


---

### Issue 2️⃣ {Issue Title}
{follow same structure as Issue 1}

---

## 📊 Summary

| Metric | Value |
|--------|-------|
| Total Issues | {count} |
| Verified ✅ | {valid_count} |
| Partially Valid ⚠️ | {partial_count} |
| Fixed ❌ | {invalid_count} |
| Similar Patterns Found | {total_patterns} |
```

## Behavior Rules

- **Multi-Language Support**: Auto-detect Python, Java, TypeScript, JavaScript, Go, C#, Ruby, Rust, C++, PHP, Kotlin
- **Evidence-Based Verification**: Only include verified issues with actual code references from the current codebase
- **Pattern Discovery**: Search for similar patterns across the codebase with exact code locations and line numbers
- **Pattern Prioritization**: Prioritize patterns appearing 3+ times, highlight when no patterns found
- **Metadata Required**: For each issue include: status, priority, category (Design/API/Documentation/Performance/Security/etc.)
- **Code References**: Use format `{line-start}:{line-end}:{filepath}` for precise code location tracking
- **Status Indicators**: Use visual markers for quick scanning
  - ✅ Valid: Issue confirmed in current codebase
  - ⚠️ Partially Valid: Issue exists with modifications or mixed findings
  - ❌ Invalid: Issue fixed or doesn't exist
  - 🔄 Modified: Code changed but concern may still apply
- **Summary Statistics**: Include validation counts and priority breakdown in final summary table
- **Report Persistence**: MUST save to `.specify/reviews/codereview-check-{component-name}-{timestamp}.md`
- **No Code Modification**: Analysis and validation only - do not modify source files
- **Deduplication**: Avoid listing same location multiple times in similar patterns
- **Visual Hierarchy**: Use emoji numbering (1️⃣ 2️⃣ 3️⃣) and markdown tables for clarity
- **🚨 CONCISE CODE SNIPPETS**: All code snippets MUST be 5-15 lines max. Show only the problematic section. Use `// ... (rest of code)` or `# ... (rest of code)` for omitted sections. This prevents oversized report files.

---

Context for verification: $ARGUMENTS
