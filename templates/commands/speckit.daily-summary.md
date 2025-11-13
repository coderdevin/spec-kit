---
description: Generate objective daily summary from work logs, matching historical report style.
---

User input: $ARGUMENTS

**Goal**: Analyze provided content and generate goal-based daily summary saved to `.specify/daily-report/YYYY-MM-DD/{model}.md`.

---

## Execution Steps

### 1. Setup

Run `date +%Y-%m-%d` to get today's date as `$TODAY`

Detect AI model and store as `$MODEL`:
- Claude Sonnet → `sonnet`
- Cursor Composer → `composer` 
- Gemini → `gemini`
- GPT-5 → `gpt5`
- Other → lowercase name

### 2. Parse Input

Accept file paths or direct text from `$ARGUMENTS`:
- Work logs, commits, code changes
- Meeting notes, progress updates
- Completed tasks, blockers

### 3. Learn Style from History

Search `.specify/daily-report/` for last 3-7 reports:
- Extract tone: objective, factual, direct
- Language: match historical (Chinese/English)
- No AI phrases: avoid "successfully", "efficiently", "great", "amazing"

**Style examples**:
- ✅ "Completed authentication flow"
- ❌ "Successfully delivered amazing authentication"
- ✅ "Fixed 3 bugs in payment module"
- ❌ "Efficiently resolved important issues"

### 4. Generate Report

```markdown
# Daily Report - [Date]

## Goal 1: [Name] - [Completed/In Progress/Blocked]
- [Outcome/deliverable]
- **Impact**: [Factual impact]
- **Blockers**: [Specific blocker or "None"]

## Goal 2: [Name] - [Completed/In Progress/Blocked]
- [Outcome/deliverable]
- **Impact**: [Factual impact]
- **Blockers**: [Specific blocker or "None"]

## Tomorrow's Focus
1. [Primary task]
2. [Secondary task]
```

---

## Rules

**Files**:
- Save to: `.specify/daily-report/$TODAY/$MODEL.md`
- Example: `.specify/daily-report/2025-11-05/sonnet.md`
- Same model/day = overwrite

**Content**:
- Group work by goals with status
- Bullet points only, no paragraphs
- Include metrics/data when available
- Max 30-40 lines total
- Objective tone, no embellishment
- Match historical report language

**Avoid**:
- Motivational phrases
- Superlatives ("amazing", "excellent")
- Filler words ("successfully", "efficiently")
- Process descriptions
- Vague statements
