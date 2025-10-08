This is the latest set of releases that you can use with your agent of choice. We recommend using the Specify CLI to scaffold your projects, however you can download these independently and manage them yourself.

## What's New in v0.0.19

### Added

- **New `/speckit.discussion` command**: Orchestrates collaborative technical discussion with 8 legendary tech experts (Linus Torvalds, James Gosling, Robert C. Martin, Martin Fowler, Jeff Dean, Werner Vogels, Rod Johnson, Eric Evans) to analyze code, architecture, and design decisions through multiple expert perspectives.
  - Supports multiple scenarios: Code Review, Architecture Design, Refactoring Review, Performance Optimization, Technology Selection, Bug Root Cause Analysis, and Technical Problem Solving.

- **New `/speckit.codereview` command**: Orchestrates comprehensive code review with 8 specialized code review experts (Coding Standards, Function Review, Class Design, Exception Handling, Testing, Architecture Design, Performance & Database, Common Library) to analyze code quality, design patterns, and best practices.
  - Provides detailed issue identification, improvement recommendations with code examples, expert round-table discussion, and prioritized actionable recommendations (High/Medium/Low priority).

- Enhanced CLI output to display both discussion and codereview commands in the enhancement commands section.
- Comprehensive documentation in README for when and how to use the discussion and codereview features.

## Installation

Install the Specify CLI:

```bash
uvx --from specify-cli specify init <project-name> --ai <agent>
```

Or download the appropriate template zip for your agent and script type.

## Supported AI Agents

- GitHub Copilot
- Claude Code
- Cursor
- Gemini CLI
- Qwen Code
- opencode
- Windsurf
- Codex CLI
- Kilo Code
- Auggie CLI
- Roo Code
- Amazon Q Developer CLI