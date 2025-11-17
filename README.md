# Ultra‑Strict Python Project Template

Maximum strictness for Python projects, inspired by TypeScript’s `--strict` mode.

This repo provides a **`pyproject.toml` template** and tooling configuration so you can:

- **Start new projects** in strict mode by copying the template.
- **Upgrade existing projects** by reusing only the pieces you need.

The goal is **production‑grade quality** from day one: strong typing, clean style, security checks, and good test coverage.

---

## Features

- **Python**: 3.12+
- **Dependency manager**: `uv`
- **Type checker**: `basedpyright` (strict mode)
- **Linter / formatter**: `ruff` (with many plugins enabled)
- **Config & env**: `python-dotenv`, `typing-extensions`
- **Data validation**: `pydantic>=2`
- **Task runner**: `poethepoet` (format, check, metrics, etc.)
- **Code quality tools**: `radon`, `skylos`
- **Testing & coverage**: `pytest` + `coverage` (80% minimum by default)

Everything is wired through **`pyproject.toml`** so you have a single source of truth.

---

## Using this for a New Project

1. **Create a new directory**:

   ```bash
   mkdir my-project
   cd my-project
   ```

2. **Copy the template `pyproject.toml`** from this repo and adapt it:

   - Change `[project]` fields:
     - `name = "my-project"`
     - `description = "My strict Python project"`
     - `authors = [...]`
   - Update `tool.ruff.lint.isort.known-first-party` to match your package name, e.g.:

     ```toml
     known-first-party = ["my_project"]
     ```

3. **Create the basic structure**:

   ```bash
   mkdir -p src/my_project tests
   touch src/my_project/__init__.py
   touch tests/__init__.py
   ```

4. **Create and activate a virtual environment (with `uv`)**:

   ```bash
   uv venv
   # Windows
   .venv\Scripts\activate
   # Linux / macOS
   # source .venv/bin/activate
   ```

5. **Install dependencies (including dev tooling)**:

   ```bash
   uv pip install -e ".[dev]"
   ```

You now have a new project in **strict mode**.

---

## Using this with an Existing Project

You don’t have to adopt everything at once. For an existing codebase you can **gradually** copy pieces of this template:

1. **Back up your current `pyproject.toml`** (or `setup.cfg`, `requirements.txt`, etc.).

2. **Copy selected sections** from this template `pyproject.toml`:

   - `[project]` (or integrate relevant fields into your existing project metadata).
   - `[project.optional-dependencies].dev` (to add `basedpyright`, `ruff`, `poethepoet`, etc.).
   - `[tool.ruff]` and `[tool.ruff.*]` blocks to adopt strict linting/formatting.
   - `[tool.basedpyright...]` block to enable strict type checking.
   - `[tool.pytest.ini_options]` and `[tool.coverage.*]` to standardize tests/coverage.

3. **Simplify if needed**:

   - You can **relax some rules** initially (e.g. unused variables, unknown types) and then tighten them once the code is cleaner.
   - You can **exclude noisy directories** (generated code, legacy folders) in the `exclude` lists for `ruff`, `basedpyright`, and coverage.

4. **Install the dev dependencies** in your existing project:

   ```bash
   uv pip install -e ".[dev]"
   ```

5. **Run the tools and fix issues iteratively** (see commands below).

---

## Commands & Workflow

You can use the raw tools or the `poethepoet` tasks defined in `pyproject.toml`.

### Daily workflow (direct commands)

```bash
# Format code
uv run ruff format .

# Lint and auto-fix
uv run ruff check . --fix

# Strict type checking
uv run basedpyright --log-level error

# Run tests with coverage
uv run pytest
```

### Poe tasks

```bash
# Format, lint (with auto-fix), and strict type check
uv run poe format

# Lint + strict type check (no auto-fix)
uv run poe check

# Lint only (with auto-fix)
uv run poe lint

# Lint with unsafe fixes
uv run poe lint-unsafe

# Quality metrics (vulture + radon)
uv run poe metrics

# Full quality pipeline (format + lint + type check + metrics)
uv run poe quality
```

These tasks are configured in `[tool.poe.tasks]` in `pyproject.toml` and you can customize them as needed.

---

## Strictness Philosophy

This template targets **maximum strictness**, roughly equivalent to:

- TypeScript `--strict`
- Python with `basedpyright` in `strict` mode
- Aggressive linting via `ruff` with many plugins enabled

That means:

- No implicit `Any` (unknown/untyped values are treated as errors).
- Required type annotations on function parameters and return types.
- Unused imports, variables, functions, and classes are treated as errors.
- Optional values (`None`) must be handled explicitly.
- Security‑oriented checks via `flake8-bandit` rules.
- Coverage threshold enforced by default (80%).

If this is too strict for an existing project, you can **progressively relax** some `basedpyright` or `ruff` rules and then turn them back on as your code improves.

---

## VS Code Setup (Recommended)

In a project using this template, a typical `.vscode/settings.json` would:

- Point VS Code to `.venv` as the default interpreter.
- Enable strict type checking.
- Use `ruff` as formatter and linter.
- Run organize‑imports and fix‑all on save.

You can copy the suggested settings from the original template comments or adapt them to your editor of choice.

---

## When to Use This Template

- **Greenfield projects** where you want high confidence and maintainability.
- **Existing projects** that you want to gradually migrate to a stricter, safer style.
- **Libraries/SDKs** that will be consumed by others and must provide strong type guarantees.

If you want to relax the rules (e.g., for experimental code or quick scripts), you can:

- Lower coverage thresholds.
- Downgrade some `basedpyright` diagnostics from errors to warnings.
- Disable specific `ruff` rules or directories.

---

## Next Steps

- Adjust `[project]` metadata to match your package.
- Update `known-first-party` in the `ruff` config.
- Add your source code under `src/` and tests under `tests/`.
- Run `uv run poe quality` before every commit or CI run.

This template is designed so you can **copy it wholesale** for new projects or **cherry‑pick** sections for older ones, while keeping the same strict philosophy throughout your Python codebase.
