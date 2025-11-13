---
description: Generate a focused daily plan by analyzing your knowledge base to prioritize tasks and create a clear execution roadmap.
---

User input: $ARGUMENTS

**Goal**: Analyze knowledge base to surface relevant work, prioritize tasks, and generate an actionable daily plan saved to `.specify/daily-plan/YYYY-MM-DD/{model}.md`.

---

## Execution Steps

### 0. Setup & Date Detection

**Get Today's Date**:
```bash
# Run shell command to get precise date
date +%Y-%m-%d
```
Store as `$TODAY` (format: `YYYY-MM-DD`, e.g., `2025-11-05`)

**Detect AI Model**:
Analyze environment/context to identify the current AI model:
- **Claude Sonnet** → `sonnet`
- **Cursor Composer** → `composer` (NOT sonnet!)
- **Gemini/Gemini Pro** → `gemini`
- **GPT-5** → `gpt5`
- **Claude Haiku** → `haiku`
- **Other models** → use simplified lowercase name

Store as `$MODEL`

### 1. Knowledge Base Discovery

**Priority Sources** (check in order):
1. **Daily reports**: Find date-formatted files (`202x-xx-xx.md`) - extract uncompleted tasks, blockers, carryovers
2. **Active specs**: `.specify/specs/*/spec.md`, `**/plan.md`
3. **Task lists**: `tasks.md`, `TODO.md`, unchecked items
4. **Recent work**: Git commits, open PRs, WIP branches
5. **Deadlines**: Date mentions, milestone keywords
6. **Blockers**: Search for "blocked", "waiting", "pending"

### 2. Task Analysis & Prioritization

Categorize by priority:
- **P0 - Critical**: Blockers, urgent deadlines, production issues
- **P1 - High**: Important with near-term deadlines
- **P2 - Medium**: Valuable without immediate pressure
- **P3 - Low**: Nice-to-have, learning, exploration

Consider: dependencies, impact, effort, context alignment.

### 3. Generate Daily Plan

Output structure:

```markdown
# Daily Plan - [Date]

## Top Priority
- [ ] **[Task]** - [Brief rationale]
- [ ] **[Task]** - [Brief rationale]

## Today's Tasks

### High Priority (P1)
- [ ] **[Task]** - [Brief description]

### Medium Priority (P2)
- [ ] **[Task]** - [Brief description]

### Low Priority (P3)
- [ ] **[Task]** - [Brief description]

## Blockers & Waiting
- [ ] **[Blocker/Waiting Item]** - [What's needed to unblock]

## Key Notes
- [ ] [Important context point]
- [ ] [Important context point]

## Summary
- Total tasks: [X]
- Focus theme: [One sentence]
```

---

## Rules

**File Management**:
- Run shell command `date +%Y-%m-%d` to get precise date
- Create directory structure: `.specify/daily-plan/$TODAY/` (e.g., `.specify/daily-plan/2025-11-05/`)
- Detect AI model accurately (see Step 0 for detection rules)
- Save to: `.specify/daily-plan/$TODAY/$MODEL.md`
- Examples:
  - `.specify/daily-plan/2025-11-05/sonnet.md`
  - `.specify/daily-plan/2025-11-05/composer.md`
  - `.specify/daily-plan/2025-11-05/gemini.md`
- Same model on same day = overwrite; different models = separate files
- Each date gets its own folder - keep directories clean and organized

**Output**:
- Max 8-10 tasks (realistic capacity)
- Minimal, actionable, scannable format
- Extract continuity from recent daily reports
- Group related tasks, highlight dependencies
- Think in English, output in user's language (default: English)
- No filler, motivational content, or excessive details
