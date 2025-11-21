---
description: Review all model-generated daily plans for today, select the best tasks from each, and generate a finalized daily plan with time schedule.
---

User input: $ARGUMENTS

**Goal**: Find all daily plans for today from different models, review checked/selected tasks, and generate a finalized consolidated plan with time schedule saved to `.specify/daily-plan/YYYY-MM-DD/final.md`.

---

## Execution Steps

### 1. Locate Today's Daily Plans

**Get Today's Date**:
```bash
# Run shell command to get precise date
date +%Y-%m-%d
```
Store as `$TODAY` (format: `YYYY-MM-DD`, e.g., `2025-11-05`)

**Target directory**: `.specify/daily-plan/$TODAY/`

**File pattern**: All `*.md` files in today's folder
- Examples: 
  - `.specify/daily-plan/2025-11-05/sonnet.md`
  - `.specify/daily-plan/2025-11-05/composer.md`
  - `.specify/daily-plan/2025-11-05/gemini.md`

**Actions**:
- Run `date +%Y-%m-%d` to get precise today's date
- Navigate to `.specify/daily-plan/$TODAY/` directory
- Find all `*.md` files (excluding `final.md` if it exists)
- If directory doesn't exist or no files found, report "No daily plans found for today"

### 2. Extract Selected Tasks

**Parse each found file**:
- Identify all checked task items: `- [x]` or `- [X]`
- These are tasks user has selected/approved from model suggestions
- Capture full task line including description
- Track which section each task belongs to (Top Priority, High/Medium/Low Priority, Blockers, Key Notes)
- Preserve task metadata (priority level, context)

**Aggregation**:
 - Normalize task text (trim whitespace, collapse multiple spaces, ignore case) to generate a deduplication key
 - Merge selected tasks from all model versions using the deduplication key
 - When tasks have the same goal but slightly different wording, consolidate into a single entry using the clearest phrasing and note all contributing models
 - Maintain original priority categorization for the merged task
- This creates a "best of all models" finalized plan

### 3. Generate Finalized Plan with Time Schedule

Output structure:

```markdown
# Daily Plan - [Date] (Finalized)

## 📅 Time Schedule

### Morning (9:10 AM - 12:00 PM)
- **9:10 - 10:30** — [High priority task block]
- **10:30 - 11:50** — [Focused work block]

### Afternoon (1:30 PM - 4:00 PM)
- **1:30 - 3:00** — [Main task block]
- **3:00 - 4:00** — [Task completion block]

### Daily Summary (5:00 PM - 6:00 PM)
- **5:00 - 6:00** — 📝 Daily summary & reflection (Fixed)

---

## Top Priority
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

## Summary
- Total tasks: [X]
- Focus theme: [One sentence]
- Compiled from: [List of model versions analyzed, e.g., "sonnet, composer, gemini"]
```

**Time Schedule Rules**:
- **Morning blocks**: 9:10-10:30 (80min), 10:30-11:50 (80min)
- **Afternoon blocks**: 1:30-3:00 (90min), 3:00-4:00 (60min)
- **5pm-6pm**: Fixed daily summary time (always include this)
- Assign high-priority tasks to morning blocks when possible
- Keep schedule simple and clear - just suggest rough time allocations
- Adjust time blocks based on task complexity and priorities
- Use emoji sparingly for visual clarity (📅 📝)

**Notes**:
- Reset all checkboxes to `- [ ]` (unchecked) in the final plan
- Only include sections with selected items
- This becomes the official plan to execute

---

## Rules

**File Management**:
- Run shell command `date +%Y-%m-%d` to get precise date
- Read from `.specify/daily-plan/$TODAY/*.md` (all model versions in today's folder)
- Exclude `final.md` from source files (don't merge final into itself)
- Save finalized plan to `.specify/daily-plan/$TODAY/final.md`
- Create directory structure if needed
- Overwrite existing final plan if regenerating
- Examples:
  - Source: `.specify/daily-plan/2025-11-05/sonnet.md`, `composer.md`, `gemini.md`
  - Output: `.specify/daily-plan/2025-11-05/final.md`

**Processing**:
- Extract tasks marked as checked: `- [x]` or `- [X]` (user-selected items)
- Ignore unchecked items: `- [ ]` (not selected by user)
- Merge and deduplicate tasks from all model versions
- Reset all checkboxes to unchecked `- [ ]` in final output
- Preserve task formatting and descriptions
- Note which models contributed tasks

**Time Schedule**:
- Always include time schedule section at the top
- Morning: 9:10-10:30 (80min), 10:30-11:50 (80min)
- Afternoon: 1:30-3:00 (90min), 3:00-4:00 (60min)
- 5pm-6pm: Fixed daily summary time (mandatory)
- Distribute selected tasks across time blocks intelligently
- High priority → morning blocks; medium/low → afternoon blocks
- Keep time suggestions simple and flexible

**Output**:
- Minimal, clean format with time schedule + task list structure
- This is the executable plan, not a summary
- Group by original priority levels
- Include source attribution (e.g., "Compiled from: sonnet, composer, gemini")
- Think in English, output in user's language (default: English)
- No motivational content or filler text

