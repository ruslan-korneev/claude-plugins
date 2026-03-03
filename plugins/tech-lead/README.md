# tech-lead

Architecture design, code review (Review Pyramid), full-cycle TDD development, and structured feature specifications. Consolidated from software-architect, code-review, full-cycle-dev, and feature-list plugins.

## Commands

### Architecture

| Command | Description |
|---------|-------------|
| `/design <name>` | Interactive architecture design for new project or module |
| `/modernize [path]` | Analyze legacy code and create phased modernization plan |
| `/arch:review [path]` | Analyze project architecture (structure, dependencies, patterns) |
| `/adr <title>` | Create Architecture Decision Record |
| `/diagram <type> [path]` | Generate architecture diagram in Mermaid format |
| `/deps [path]` | Analyze module dependencies, detect circular dependencies |

### Code Review

| Command | Description |
|---------|-------------|
| `/review [target] [--task] [--mode]` | Review code changes using the Review Pyramid methodology |

### Development

| Command | Description |
|---------|-------------|
| `/dev <prompt> [--module]` | Full TDD development cycle (explore, plan, execute) |
| `/plan <prompt> [--module]` | Create a development plan without execution |
| `/execute <plan_path>` | Execute an existing development plan using TDD |

### Features

| Command | Description |
|---------|-------------|
| `/features:init [path]` | Initialize feature directory structure with templates |
| `/features:analyze [path] [--phase]` | Analyze codebase and generate feature specifications |
| `/features:design [--type]` | Interactive feature design session for new projects |
| `/features:add <phase> <name>` | Add a new feature to a specific phase |
| `/features:graph` | Regenerate the dependency graph in README |
| `/features:status` | Show implementation status by phase |

## Agents

| Agent | Model | Purpose |
|-------|-------|---------|
| architecture-reviewer | opus | Deep architecture analysis with comprehensive checklist |
| dependency-analyzer | opus | Analyze dependencies at module, package, and layer levels |
| code-reviewer | opus | Deep code review following the Review Pyramid |
| quick-reviewer | sonnet | Fast first-pass review for automated checks |
| codebase-explorer | sonnet | Fast codebase analysis to find relevant modules |
| planning-agent | opus | Deep analysis and interactive planning for features |
| execution-agent | opus | TDD execution with Red-Green-Refactor cycle |
| codebase-analyzer | sonnet | Extract features from existing code |
| feature-designer | opus | Interactive feature design for new projects |

## Skills

| Skill | Description |
|-------|-------------|
| architecture-patterns | Layered architecture, API design, data modeling, anti-patterns, ADR templates |
| code-review | Review Pyramid methodology, API semantics, implementation, security, testing |
| tdd-workflow | TDD development cycle, plan format, context handoff, plugin integration |
| feature-specification | Feature templates, ID conventions, phases, dependency graphs |
