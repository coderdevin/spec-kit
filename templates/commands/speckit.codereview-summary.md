---
description: Consolidate and organize reports from speckit.codereview-xxx and speckit.codereview-check into a well-structured, comprehensive summary document.
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Workflow

1. **Locate Reports**: Search `.specify/reviews/` for original review and validation reports (use user-specified files or most recent)
2. **Detect Language**: Determine output language by analyzing source reports (if reports are in Chinese, generate summary in Chinese; if English, use English)
3. **Parse Reports**: Extract component names, issues, validation results, and statistics
4. **Filter & Consolidate**: 
   - Only include issues from validation report (= checked `[x]` issues only)
   - Exclude ❌ Invalid/Fixed issues
   - Merge original + validation data for each issue
5. **Generate Summary**: Create consolidated report with assessment scores in the detected language

## Core Filtering Rules 🚨

### Source of Truth
- **Validation report** (`codereview-check`) contains ONLY issues checked `[x]` by user in original review
- Use validation report as the authoritative source for which issues to include

### Inclusion Criteria
- ✅ **Valid**: Issue confirmed, must be addressed
- ⚠️ **Partially Valid**: Issue partially applies, needs consideration  
- 🔄 **Modified**: Code changed since review, re-evaluate needed

### Exclusion Criteria
- ❌ **Invalid/Fixed**: Issue not applicable or already resolved
- `[ ]` **Unchecked**: Issues not selected by user in original review (not in validation report)

### Consolidation Rules
- Merge original review + validation analysis for each issue into single entry
- No duplication of issues across categories
- Preserve exact descriptions and code snippets from source reports
- Keep code examples concise (5-15 lines, extract key sections only)
- Follow logical flow: Description → Code → Recommendation → Validation → Patterns

---

## Report Structure

### 1️⃣ Metadata Section
```markdown
## 📋 Report Metadata

| Item | Details |
|------|---------|
| **Generated** | {ISO 8601 timestamp} |
| **Component/Module** | {name} |
| **Source Reports** | {original review file, validation report file} |
| **Languages** | {detected languages} |
| **Total Issues (Original)** | {count from original review} |
| **Checked by User** | {count of [x] issues} |
| **Included in Summary** | {✅+⚠️+🔄 count} |
| **Excluded (Invalid/Fixed)** | {❌ count} |
| **Excluded (Not Checked)** | {[ ] count} |
```

### 2️⃣ Overall Assessment Section
```markdown
## 📈 Overall Assessment

**Overall Score**: {X.X}/10 {🟢/🟡/🟠/🔴}

**Summary**: {2-3 sentences on overall quality, based on consolidated findings}

### Score Breakdown by Dimension

| Dimension | Score | Issues | Notes |
|-----------|-------|--------|-------|
| Architecture & Design | {X}/10 | {count} | {brief note} |
| Code Quality & Maintainability | {X}/10 | {count} | {brief note} |
| Error Handling & Robustness | {X}/10 | {count} | {brief note} |
| Performance & Efficiency | {X}/10 | {count} | {brief note} |
| Security & Safety | {X}/10 | {count} | {brief note} |
| Testing & Testability | {X}/10 | {count} | {brief note} |
| Documentation & Clarity | {X}/10 | {count} | {brief note} |
| Best Practices & Standards | {X}/10 | {count} | {brief note} |
```

### 3️⃣ Review Findings Section
```markdown
## 🔍 Review Findings

### {Category Name}

#### Issue 1️⃣ {Title}

**Priority**: {🔴/🟠/🟡/🟢} | **Category**: {type} | **Status**: {✅/⚠️/🔄}  
**Location**: `{filepath}:{line-start}-{line-end}`

**Description**: {original issue description}

**Code**:
```{language}
{concise code snippet 5-15 lines}
```

**Recommendation**: {solution from original report}

**Validation**: {analysis from validation report}

**Similar Patterns**: {count} occurrences
1. `{filepath}:{lines}` - {brief description}
2. `{filepath}:{lines}` - {brief description}

---
```

## Assessment & Scoring Guide

### Scoring Methodology
**Base Principle**: Analyze issues from both original review AND validation report comprehensively.

#### Dimension Mapping
Map issue categories to 8 assessment dimensions:
- **Architecture & Design**: Design patterns, modularity, coupling, SOLID principles
- **Code Quality & Maintainability**: Code clarity, duplication, complexity, naming
- **Error Handling & Robustness**: Exception handling, null checks, edge cases
- **Performance & Efficiency**: Algorithmic efficiency, resource usage, optimization
- **Security & Safety**: Vulnerabilities, data validation, authentication
- **Testing & Testability**: Test coverage, unit tests, mocking capabilities
- **Documentation & Clarity**: Code comments, API documentation, inline explanations
- **Best Practices & Standards**: Language conventions, industry standards, framework patterns

#### Scoring Formula
1. **Start**: 10/10 for each dimension
2. **Deduct per issue**: `severity_weight × frequency_factor`
   - Severity weights: Critical=2.0, High=1.5, Medium=1.0, Low=0.5
   - Frequency factor: 1.0 + (similar_patterns × 0.2), max 2.0
3. **Minimum**: 1/10 per dimension
4. **Overall**: Weighted average of all dimensions, rounded to 1 decimal

#### Score Interpretation
- **9.0-10.0** 🟢 Excellent: Minimal issues, best practices followed
- **7.0-8.9** 🟡 Good: Minor issues, generally solid implementation
- **5.0-6.9** 🟠 Fair: Moderate issues, needs improvement
- **1.0-4.9** 🔴 Needs Improvement: Significant issues, requires attention



## Execution Requirements

### File Operations
- **Save Location**: `.specify/reviews/codereview-summary-{component}-{timestamp}.md`
- **Timestamp Format**: ISO 8601 (YYYY-MM-DD HH:MM:SS)
- **Read-Only**: Do not modify source reports or code files

### Content Processing
- **Output Language**: Detect from source reports content - if original review and validation reports are in Chinese, generate summary in Chinese; if in English, use English. Match the language of the source reports.
- **Programming Language Detection**: Auto-detect from file extensions and code blocks
- **Code Examples**: Keep concise (5-15 lines), extract key problematic sections only
- **Deduplication**: Consolidate same issue from multiple reports into single entry
- **Empty Sections**: Omit sections with no content instead of showing placeholders
- **Pattern Tracking**: Identify high-frequency patterns (≥3 occurrences)

### Quality Standards
- **Objective Assessment**: Base all scores on concrete evidence from reports
- **No Interpretation**: Preserve exact descriptions and recommendations from source reports
- **Professional Formatting**: Use proper markdown syntax, aligned tables, language-tagged code blocks
- **Complete Integration**: Each issue includes description + code + recommendation + validation + patterns
- **Error Handling**: Document missing/unreadable reports clearly

---

**Context**: $ARGUMENTS