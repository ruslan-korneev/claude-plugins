# Python Plugin

Consolidated Python code quality plugin. Merges ruff-lint, python-typing, clean-code, and pytest-assistant into a single plugin.

**Principles**: ZERO `noqa`, ZERO `type: ignore`, TDD-first, SOLID.

## Commands

### Linting (Ruff)
- `/lint` -- run ruff check on code
- `/lint:config` -- configure ruff for the project
- `/lint:explain` -- explain a ruff error with a proper fix

### Type Checking
- `/typecheck` -- run mypy/pyright
- `/typecheck:explain` -- explain a type error with a proper fix

### Clean Code
- `/clean:review` -- analyze code for smells and principle violations
- `/clean:refactor` -- suggest refactoring for a specific smell

### Testing (Pytest)
- `/test:first` -- create a test before implementation (TDD red phase)
- `/test:fixture` -- create a pytest fixture
- `/test:mock` -- create a mock for an external dependency

## Skills

- **ruff-patterns** -- ruff configuration, rule solutions, why-no-noqa philosophy
- **typing-patterns** -- dict typing, generics, why-no-type-ignore philosophy
- **clean-code-patterns** -- code smells, naming, SOLID principles
- **pytest-patterns** -- fixtures, mocking, FastAPI testing

## Agents

- **test-reviewer** -- coverage and test quality analysis

## Hooks

PostToolUse on Edit/Write of `.py` files: auto-runs `ruff format`, `ruff check --fix`, and `mypy`.
