# Changelog

<!-- markdownlint-disable MD024 -->

All notable changes to the Specify CLI and templates are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.0.31] - 2025-11-21

### Enhanced

- **Code Review PR Command Optimization**: Refactored `/speckit.codereview-pr` command template for better token efficiency, stricter output formatting (3-section report), and consolidated role definitions.
- **Daily Plan Merge Improvement**: Enhanced `/speckit.daily-plan-merge` with smarter task deduplication logic that normalizes text and consolidates similar tasks from different models.

## [0.0.30] - 2025-11-20

### Added

- **Code Review PR Support**: New `/speckit.codereview-pr` command that orchestrates comprehensive code reviews by combining Linear issue requirements with GitHub Pull Request diffs.

## [0.0.29] - 2025-10-31

### Changed

- Release automation and package management improvements

## [0.0.28] - 2025-10-30

### Added

- **Vue 2.7 Code Review Support**: New `/speckit.codereview-vue2` command for comprehensive Vue 2.7 frontend code review with 6 specialized experts analyzing component architecture, Composition API/Options API patterns, state management (Vuex/Pinia), performance optimization, testing quality, security vulnerabilities, accessibility, and CSS architecture.

### Enhanced

- **Discussion Command Optimization**: Improved `/speckit.discussion` command with dynamic expert selection (2-8 experts based on problem complexity), complexity assessment system, and streamlined round table discussion format. Now includes transparent expert selection rationale and optimized for simple (2-3 experts), medium (4-5 experts), and complex (6-8 experts) technical problems.

## [0.0.27] - 2025-10-29

### Added

- **Code Review Framework Expansion**: Added comprehensive language-specific code review commands:
  - New `/speckit.codereview-java` command for Java-specific code review with 8 specialized experts
  - New `/speckit.codereview-python` command for Python-specific code review with 8 specialized experts
  - New `/speckit.codereview-summary` command for generating executive summaries from multi-file code review sessions
- **Documentation**: Added comprehensive code review tutorials and quick references:
  - `docs/code-review-tutorial.md` - Complete guide on using the code review system
  - `docs/code-review-tutorial-zh.md` - Chinese version of the code review tutorial
  - `docs/code-review-quick-reference.md` - Quick reference for code review commands

### Changed

- Removed deprecated `/speckit.codereview` general command (replaced by language-specific variants)
- Updated documentation table of contents to include new code review guides

## [0.0.26] - 2025-10-28

### Enhanced

- **Frontend Code Review Enhancement**: Significantly improved `/speckit.codereview-react` command with expanded review coverage:
  - Enhanced expert roles from 6 to cover 10 comprehensive review dimensions
  - Added Security & Accessibility Expert (Sub Agent 6) combining frontend security vulnerability detection with web accessibility audit
  - Added Testing Quality & Code Standards Expert (Sub Agent 5) for test coverage, React Testing Library best practices, and clean code principles
  - Enhanced Performance Expert (Sub Agent 4) to include CSS Architecture & Styling review covering CSS-in-JS, CSS Modules, Tailwind CSS, responsive design, design systems, and animation performance
  - Expanded Component Architecture Expert (Sub Agent 1) to include testability design, HOC/Render Props patterns, and component API design
  - Enhanced Hooks Expert (Sub Agent 2) to cover comprehensive state management patterns including Redux/Zustand/Jotai/Recoil, Context API, server vs client state
  - Enhanced TypeScript Expert (Sub Agent 3) with advanced patterns including conditional/mapped types, discriminated unions, and React-specific types
  - Added critical "Problematic Code Requirement" rule requiring actual code snippets in all issue identifications with minimum 3-5 lines context
  - Enhanced priority system to include security vulnerabilities (XSS, injection attacks, exposed secrets) as Critical (P0) items
  - Streamlined round table discussion format for better actionable insights
  - Simplified comprehensive review report with clearer score breakdown across 10 dimensions
  - Updated behavior rules to emphasize evidence-based reviews with mandatory code evidence quality standards

## [0.0.25] - 2025-10-28

### Fixed

- **CRITICAL**: Fixed missing `speckit.` prefix on all command template files to ensure proper discoverability. All command templates now follow the naming convention `speckit.{command-name}.md` as required by version 0.0.18 standards:
  - Renamed `codereview-check.md` → `speckit.codereview-check.md`
  - Renamed `codereview-react.md` → `speckit.codereview-react.md`
  - Renamed `analyze.md` → `speckit.analyze.md`
  - Renamed `api-spec-audit.md` → `speckit.api-spec-audit.md`
  - Renamed `api-spec-generator.md` → `speckit.api-spec-generator.md`
  - Renamed `checklist.md` → `speckit.checklist.md`
  - Renamed `clarify.md` → `speckit.clarify.md`
  - Renamed `codereview.md` → `speckit.codereview.md`
  - Renamed `constitution.md` → `speckit.constitution.md`
  - Renamed `discussion.md` → `speckit.discussion.md`
  - Renamed `implement.md` → `speckit.implement.md`
  - Renamed `job-analysis.md` → `speckit.job-analysis.md`
  - Renamed `plan.md` → `speckit.plan.md`
  - Renamed `specify.md` → `speckit.specify.md`
  - Renamed `tasks.md` → `speckit.tasks.md`
  - Renamed `unittest-java.md` → `speckit.unittest-java.md`

This ensures all commands are now accessible via `/speckit.` prefix in AI agents (Cursor, Claude Code, GitHub Copilot, etc.).

- Fixed release package script (`.github/workflows/scripts/create-release-packages.sh`) to prevent duplicate `speckit.` prefix in generated command filenames. The script no longer adds an additional `speckit.` prefix since template files already include it.

## [0.0.24] - 2025-10-28

### Added

- New `/speckit.codereview-check` command for validating checked issues from code review reports, discovering similar patterns across the codebase, and generating comprehensive problem analysis documents with concrete solutions.
- New `/speckit.codereview-react` command for orchestrating comprehensive frontend code review with 6 specialized React and TypeScript experts analyzing component design, hooks, state management, performance, accessibility, and best practices.
- `.cursorrules` configuration file to optimize Cursor IDE experience by preventing automatic loading of large template files.

## [0.0.23] - 2025-10-09

### Added

- New `/speckit.job-analysis` command for deep analysis of Python job/task code with comprehensive specification generation including business workflows (Mermaid diagrams), issue detection, status determination logic, performance analysis, and prioritized improvement recommendations.

## [0.0.22] - 2025-10-09

### Changed

- **BREAKING**: Renamed `/speckit.api-spec` to `/speckit.api-spec-generator` for clarity of purpose (generates API specifications from requirements).
- **BREAKING**: Renamed `/speckit.api-spec-review` to `/speckit.api-spec-audit` to better reflect automated quality checking (360-item comprehensive audit checklist).
- Fixed markdown formatting issues in `api-spec-generator.md` and `api-spec-audit.md` (removed nested code blocks, standardized table formatting).
- Updated all internal references and documentation to use new command names.
- Enhanced `api-spec-generator.md` template to align with production-grade API documentation standards, including process steps, business rules, and lifecycle-based pseudo code.

## [0.0.21] - 2025-10-08

### Changed

- Allow overriding the template download repository via the `SPECIFY_TEMPLATE_REPO` environment variable and default to `coderdevin/spec-kit` so newly added commands are available when initializing projects.
- Updated CLI onboarding panels and README to include the new `/speckit.unittest-java` command for disciplined JUnit 5 generation.

## [0.0.19] - 2025-10-08

### Added

- New `/speckit.discussion` command template that orchestrates collaborative technical discussion with 8 legendary tech experts (Linus Torvalds, James Gosling, Robert C. Martin, Martin Fowler, Jeff Dean, Werner Vogels, Rod Johnson, Eric Evans) to analyze code, architecture, and design decisions through multiple expert perspectives.
- Discussion command supports multiple scenarios: Code Review, Architecture Design, Refactoring Review, Performance Optimization, Technology Selection, Bug Root Cause Analysis, and Technical Problem Solving.
- New `/speckit.codereview` command template that orchestrates comprehensive code review with 8 specialized code review experts (Coding Standards, Function Review, Class Design, Exception Handling, Testing, Architecture Design, Performance & Database, Common Library) to analyze code quality, design patterns, and best practices.
- Code review command provides detailed issue identification, improvement recommendations with code examples, expert round-table discussion, and prioritized actionable recommendations (High/Medium/Low priority).
- Enhanced CLI output to display both discussion and codereview commands in the enhancement commands section.
- Comprehensive documentation in README for when and how to use the discussion and codereview features.

## [0.0.18] - 2025-10-06

### Added

- Support for using `.` as a shorthand for current directory in `specify init .` command, equivalent to `--here` flag but more intuitive for users.
- Use the `/speckit.` command prefix to easily discover Spec Kit-related commands.
- Refactor the prompts and templates to simplify their capabilities and how they are tracked. No more polluting things with tests when they are not needed.
- Ensure that tasks are created per user story (simplifies testing and validation).
- Add support for Visual Studio Code prompt shortcuts and automatic script execution.

### Changed

- All command files now prefixed with `speckit.` (e.g., `speckit.specify.md`, `speckit.plan.md`) for better discoverability and differentiation in IDE/CLI command palettes and file explorers

## [0.0.17] - 2025-09-22

### Added

- New `/clarify` command template to surface up to 5 targeted clarification questions for an existing spec and persist answers into a Clarifications section in the spec.
- New `/analyze` command template providing a non-destructive cross-artifact discrepancy and alignment report (spec, clarifications, plan, tasks, constitution) inserted after `/tasks` and before `/implement`.
	- Note: Constitution rules are explicitly treated as non-negotiable; any conflict is a CRITICAL finding requiring artifact remediation, not weakening of principles.

## [0.0.16] - 2025-09-22

### Added

- `--force` flag for `init` command to bypass confirmation when using `--here` in a non-empty directory and proceed with merging/overwriting files.

## [0.0.15] - 2025-09-21

### Added

- Support for Roo Code.

## [0.0.14] - 2025-09-21

### Changed

- Error messages are now shown consistently.

## [0.0.13] - 2025-09-21

### Added

- Support for Kilo Code. Thank you [@shahrukhkhan489](https://github.com/shahrukhkhan489) with [#394](https://github.com/github/spec-kit/pull/394).
- Support for Auggie CLI. Thank you [@hungthai1401](https://github.com/hungthai1401) with [#137](https://github.com/github/spec-kit/pull/137).
- Agent folder security notice displayed after project provisioning completion, warning users that some agents may store credentials or auth tokens in their agent folders and recommending adding relevant folders to `.gitignore` to prevent accidental credential leakage.

### Changed

- Warning displayed to ensure that folks are aware that they might need to add their agent folder to `.gitignore`.
- Cleaned up the `check` command output.

## [0.0.12] - 2025-09-21

### Changed

- Added additional context for OpenAI Codex users - they need to set an additional environment variable, as described in [#417](https://github.com/github/spec-kit/issues/417).

## [0.0.11] - 2025-09-20

### Added

- Codex CLI support (thank you [@honjo-hiroaki-gtt](https://github.com/honjo-hiroaki-gtt) for the contribution in [#14](https://github.com/github/spec-kit/pull/14))
- Codex-aware context update tooling (Bash and PowerShell) so feature plans refresh `AGENTS.md` alongside existing assistants without manual edits.

## [0.0.10] - 2025-09-20

### Fixed

- Addressed [#378](https://github.com/github/spec-kit/issues/378) where a GitHub token may be attached to the request when it was empty.

## [0.0.9] - 2025-09-19

### Changed

- Improved agent selector UI with cyan highlighting for agent keys and gray parentheses for full names

## [0.0.8] - 2025-09-19

### Added

- Windsurf IDE support as additional AI assistant option (thank you [@raedkit](https://github.com/raedkit) for the work in [#151](https://github.com/github/spec-kit/pull/151))
- GitHub token support for API requests to handle corporate environments and rate limiting (contributed by [@zryfish](https://github.com/@zryfish) in [#243](https://github.com/github/spec-kit/pull/243))

### Changed

- Updated README with Windsurf examples and GitHub token usage
- Enhanced release workflow to include Windsurf templates

## [0.0.7] - 2025-09-18

### Changed

- Updated command instructions in the CLI.
- Cleaned up the code to not render agent-specific information when it's generic.


## [0.0.6] - 2025-09-17

### Added

- opencode support as additional AI assistant option

## [0.0.5] - 2025-09-17

### Added

- Qwen Code support as additional AI assistant option

## [0.0.4] - 2025-09-14

### Added

- SOCKS proxy support for corporate environments via `httpx[socks]` dependency

### Fixed

N/A

### Changed

N/A
