# Quick Start Guide

This guide will help you get started with Spec-Driven Development using Spec Kit.

> NEW: All automation scripts now provide both Bash (`.sh`) and PowerShell (`.ps1`) variants. The `specify` CLI auto-selects based on OS unless you pass `--script sh|ps`.

## The 4-Step Process

### 1. Install Specify

Initialize your project depending on the coding agent you're using:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

Pick script type explicitly (optional):
```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script ps  # Force PowerShell
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script sh  # Force POSIX shell
```

### 2. Create the Spec

Use the `/speckit.specify` command to describe what you want to build. Focus on the **what** and **why**, not the tech stack.

```bash
/speckit.specify Build an application that can help me organize my photos in separate photo albums. Albums are grouped by date and can be re-organized by dragging and dropping on the main page. Albums are never in other nested albums. Within each album, photos are previewed in a tile-like interface.
```

### 3. Create a Technical Implementation Plan

Use the `/speckit.plan` command to provide your tech stack and architecture choices.

```bash
/speckit.plan The application uses Vite with minimal number of libraries. Use vanilla HTML, CSS, and JavaScript as much as possible. Images are not uploaded anywhere and metadata is stored in a local SQLite database.
```

### 4. Break Down and Implement

Use `/speckit.tasks` to create an actionable task list, then ask your agent to implement the feature.

## Detailed Example: Building Taskify

Here's a complete example of building a team productivity platform:

### Step 1: Define Requirements with `/speckit.specify`

```text
Develop Taskify, a team productivity platform. It should allow users to create projects, add team members,
assign tasks, comment and move tasks between boards in Kanban style. In this initial phase for this feature,
let's call it "Create Taskify," let's have multiple users but the users will be declared ahead of time, predefined.
I want five users in two different categories, one product manager and four engineers. Let's create three
different sample projects. Let's have the standard Kanban columns for the status of each task, such as "To Do,"
"In Progress," "In Review," and "Done." There will be no login for this application as this is just the very
first testing thing to ensure that our basic features are set up. For each task in the UI for a task card,
you should be able to change the current status of the task between the different columns in the Kanban work board.
You should be able to leave an unlimited number of comments for a particular card. You should be able to, from that task
card, assign one of the valid users. When you first launch Taskify, it's going to give you a list of the five users to pick
from. There will be no password required. When you click on a user, you go into the main view, which displays the list of
projects. When you click on a project, you open the Kanban board for that project. You're going to see the columns.
You'll be able to drag and drop cards back and forth between different columns. You will see any cards that are
assigned to you, the currently logged in user, in a different color from all the other ones, so you can quickly
see yours. You can edit any comments that you make, but you can't edit comments that other people made. You can
delete any comments that you made, but you can't delete comments anybody else made.
```

### Step 2: Refine the Specification

After the initial specification is created, clarify any missing requirements:

```text
For each sample project or project that you create there should be a variable number of tasks between 5 and 15
tasks for each one randomly distributed into different states of completion. Make sure that there's at least
one task in each stage of completion.
```

Also validate the specification checklist:

```text
Read the review and acceptance checklist, and check off each item in the checklist if the feature spec meets the criteria. Leave it empty if it does not.
```

### Step 3: Generate Technical Plan with `/speckit.plan`

Be specific about your tech stack and technical requirements:

```text
We are going to generate this using .NET Aspire, using Postgres as the database. The frontend should use
Blazor server with drag-and-drop task boards, real-time updates. There should be a REST API created with a projects API,
tasks API, and a notifications API.
```

### Step 4: Validate and Implement

Have your AI agent audit the implementation plan:

```text
Now I want you to go and audit the implementation plan and the implementation detail files.
Read through it with an eye on determining whether or not there is a sequence of tasks that you need
to be doing that are obvious from reading this. Because I don't know if there's enough here.
```

Finally, implement the solution:

```text
implement specs/002-create-taskify/plan.md
```

## Optional Enhancement Commands

At any stage of development, you can use these optional commands to improve quality and confidence:

### `/speckit.clarify`

Ask structured questions to de-risk ambiguous areas before planning. Run before `/speckit.plan` if used.

```text
/speckit.clarify
```

### `/speckit.analyze`

Cross-artifact consistency and alignment report. Run after `/speckit.tasks`, before `/speckit.implement`.

```text
/speckit.analyze
```

### `/speckit.checklist`

Generate quality checklists to validate requirements completeness, clarity, and consistency. Run after `/speckit.plan`.

```text
/speckit.checklist
```

### `/speckit.discussion`

Orchestrate collaborative technical discussion with 8 legendary tech experts to analyze code, architecture, and design decisions. Use at any stage when you need multiple expert perspectives.

**Example usage:**

```text
/speckit.discussion Review the authentication implementation in auth.ts. 
Analyze the security approach, scalability considerations, and identify potential vulnerabilities.
```

The discussion command provides:
- Analysis from 8 expert perspectives simultaneously
- Detailed technical assessments with ratings
- Round-table discussion highlighting consensus and divergent viewpoints
- Comprehensive solution report with actionable recommendations
- Engineering feasibility and implementation complexity assessment

### `/speckit.codereview`

Orchestrate comprehensive code review with 8 specialized code review experts to analyze code quality, design patterns, and best practices. Use when you need thorough code quality assessment.

**Example usage:**

```text
/speckit.codereview Review the UserService class in src/services/user-service.ts. 
Focus on error handling, database query optimization, and testability. 
Provide prioritized recommendations for improvement.
```

The codereview command provides:
- Analysis from 8 specialized code review experts simultaneously
- Detailed issue identification with code locations and context
- Specific improvement recommendations with code examples
- Expert round-table discussion highlighting consensus and trade-offs
- Comprehensive review report with prioritized, actionable recommendations
- Code quality assessment across multiple dimensions with quantitative scoring
- Priority system: High (must fix), Medium (should fix), Low (optional optimization)

## Key Principles

- **Be explicit** about what you're building and why
- **Don't focus on tech stack** during specification phase
- **Iterate and refine** your specifications before implementation
- **Validate** the plan before coding begins
- **Let the AI agent handle** the implementation details
- **Use enhancement commands** when you need deeper analysis or validation

## Next Steps

- Read the complete methodology for in-depth guidance
- Check out more examples in the repository
- Explore the source code on GitHub
