---
description: Add notes or comments to today's daily plan final summary
---

User input: $ARGUMENTS

**Goal**: Find and add notes/comments to the final.md file in today's daily plan directory.

---

## Execution Steps

### 0. Setup & Date Detection

**Get Today's Date**:
```bash
date +%Y-%m-%d
```
Store as `$TODAY` (format: `YYYY-MM-DD`, e.g., `2025-11-13`)

**Target Directory**: `.specify/daily-plan/$TODAY/`

### 1. Locate final.md

**Find the final.md file**:
- Look in: `.specify/daily-plan/$TODAY/final.md`
- If the file doesn't exist, report the error and suggest running `/speckit.daily-plan-merge` first
- If the directory doesn't exist, report the error and suggest running `/speckit.daily-plan` first

### 2. Find Relevant Content & Add Note

**Read and analyze final.md**:
- Parse existing content structure (tasks, summaries, sections)
- Identify relevant sections that relate to the user's note
- Look for matching keywords, topics, or task items

**Process user input**:
- Take the provided `$ARGUMENTS` as the note/comment content
- Find the most relevant section or item to attach this note to

**Add contextual notes to final.md**:
- **If note relates to a specific task**: Add note directly under that task item
- **If note relates to a section**: Add note under that specific section  
- **If general note**: Add to "Notes & Comments" section
- Use visual markers and formatting to highlight notes

**Example formats**:
```markdown
## Today's Tasks
- [ ] **[Task Name]** - Task description
  > **Note:** [14:30] $ARGUMENTS

## High Priority (P1)
- [ ] **[Task Name]** - Task description
  > **🔴 Critical:** [14:30] Related progress update here

## Summary
[existing summary content...]

## Notes & Comments
> **[14:30]** [General note about today's work]

> **[15:45]** [Another general note]

> **🔴 Critical Update:** [Red-colored urgent note]
> **🟡 Important:** [Yellow-colored important note]  
> **🟢 Info:** [Green-colored informational note]

---
**💭 Note:** [16:00] Alternative format using blockquote separators
```

### 3. Save and Confirm

**Save the updated file**:
- Write the updated content back to `.specify/daily-plan/$TODAY/final.md`
- Preserve existing content while adding the new note

**Report success**:
- Confirm the note has been added
- Show the file location for reference

---

## Rules

**File Management**:
- Always run `date +%Y-%m-%d` to get current date
- Target directory: `.specify/daily-plan/$TODAY/`
- Target file: `.specify/daily-plan/$TODAY/final.md`
- Do not create new files - only modify existing final.md
- If final.md doesn't exist, inform user and suggest running `/speckit.daily-plan-merge` first

**Note Formatting**:
- Use `>` blockquote syntax for visual distinction (works in all Markdown viewers)
- Task-specific notes: `> **Note:** [HH:MM] [note content]` (properly indented)
- General notes: `> **[HH:MM]** [note content]` in "Notes & Comments" section
- Use emoji-coded prefixes for priority:
  - `> **🔴 Critical:** [urgent note]` for critical updates
  - `> **🟡 Important:** [important note]` for important information  
  - `> **🟢 Info:** [informational note]` for general information
- Alternative format: `**💭 Note:** [HH:MM] [content]` with horizontal rule separators
- Add proper markdown indentation for task-specific notes
- Find and attach notes to relevant existing content
- Preserve all existing content structure

**Error Handling**:
- If directory doesn't exist: "Daily plan directory not found. Please run `/speckit.daily-plan-merge` first."
- If final.md doesn't exist: "Final summary file not found. Please run `/speckit.daily-plan-merge` first."
- If user provides no content: "Please provide a note or comment to add."

**Output**:
- Confirm successful addition of note
- Show file location: `Note added to: .specify/daily-plan/$TODAY/final.md`
- Keep responses concise and actionable
