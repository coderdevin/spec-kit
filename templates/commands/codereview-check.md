---
description: Validate checked issues from code review reports, discover similar patterns across the codebase, and generate a comprehensive problem analysis document with concrete solutions.
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
   - Assess severity (✅ Valid / ⚠️ Partially Valid / ❌ Invalid / 🔄 Modified)
   - Collect concrete code evidence with context

4. **Discover Patterns**: For each validated issue, search codebase for similar instances:
   - Use `grep` for exact patterns (e.g., `: any`, `except:`, raw types)
   - Use `codebase_search` for semantic patterns (e.g., "large functions", "duplicated code")
   - Prioritize patterns appearing 3+ times
   - Adapt search strategies by language (see search strategies below)

5. **Generate Report**: For each verified issue, create problem analysis with:
   - Problem code with context
   - Detailed description and impact
   - 3-5 additional examples found
   - Concrete, implementable solution
   - Implementation steps
   - Impact analysis (files affected, effort, risk, benefits)

6. **Save Report**: Write to `.specify/reviews/codereview-check-{name}-{timestamp}.md` with:
   - Header: source review, statistics, languages detected
   - Problem sections (ordered by priority)
   - Summary: issues by priority, total impact, action plan, next steps

7. **Output Summary**: Report completion with statistics and recommended actions

## Report Structure

```markdown
# {Component/Module Name} - Problem Analysis Report

**Generated**: {timestamp}
**Source Review**: `.specify/reviews/{original-filename}`
**Languages**: {detected languages}
**Valid Issues**: {count} / {total checked}
**Similar Patterns**: {count} additional instances

---

## Problem {N}: {Issue Title}

**File:** `{filepath}` (Lines {start}-{end})  
**Language:** {language}

**Problem Code:**
```{language}
{actual code with context}
```

**Problem Description:**
{Impact on quality/maintainability/performance/security}

**Additional Examples:**
1. `{file}` - Lines {start}-{end}: {brief description}
2. `{file}` - Lines {start}-{end}: {brief description}
3. `{file}` - Lines {start}-{end}: {brief description}

**Solution:**
```{language}
{implementable solution}
```

**Solution Explanation:**
{Why this solves the problem, benefits, trade-offs}

**Implementation Steps:**
1. {step}
2. {step}
3. {step}

**Impact:**
- Files Affected: {count}
- Estimated Effort: {time}
- Risk Level: {Low/Medium/High}
- Benefits: {concrete benefits}

---

## Summary

### Issues by Priority
**🔴 Critical**: {count} - {list}
**🟠 High**: {count} - {list}
**🟡 Medium**: {count} - {list}
**🟢 Low**: {count} - {list}

### Total Impact
- Files Requiring Changes: {count}
- Lines Affected: ~{count}
- Estimated Effort: {time}

### Action Plan
1. **Phase 1 (Critical)**: {recommendation}
2. **Phase 2 (High)**: {recommendation}
3. **Phase 3 (Medium)**: {recommendation}
4. **Phase 4 (Low)**: {recommendation}

### Next Steps
- [ ] Review report with team
- [ ] Prioritize by business impact
- [ ] Create implementation tasks
- [ ] Assign phase owners
- [ ] Schedule code review sessions
```

## Behavior Rules

- **Multi-Language Support**: Auto-detect Python, Java, TypeScript, JavaScript, Go, C#, Ruby, Rust, C++, PHP, Kotlin
- **Evidence-Based**: Only include verified issues from actual codebase
- **Concrete Examples**: Real code snippets, not hypothetical
- **Pattern Focus**: Prioritize patterns appearing 3+ times
- **Solution-Oriented**: Every problem needs concrete, language-appropriate solution
- **Context Preservation**: Include 5-10 lines before/after in examples
- **Report Persistence**: MUST save to `.specify/reviews/codereview-check-{name}-{timestamp}.md`
- **Verification Accuracy**: Mark Invalid if issue fixed or doesn't exist
- **No Code Modification**: Analysis only - do not modify source files
- **Deduplication**: Avoid listing same location multiple times
- **Language-Aware**: Follow language-specific best practices in solutions

---

Context for verification: $ARGUMENTS