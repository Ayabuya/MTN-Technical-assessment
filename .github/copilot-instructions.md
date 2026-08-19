# Copilot Instructions for this Repository

## Project context
This repository contains a technical assessment submission for a lakehouse-oriented data engineering solution. The work spans SQL, PySpark ETL scripts, documentation, and pytest-based validation.

## General guidance
- Prefer clear, maintainable code over clever shortcuts.
- Keep changes aligned with the assessment objectives: correctness, idempotency, data quality, and explainability.
- Preserve the existing repository structure and naming conventions.
- When modifying SQL, Python, or documentation, keep the change scoped and easy to review.

## Python conventions
- Favor explicit, readable PySpark code with small, focused functions.
- Keep ETL logic in the existing module structure under src/etl/.
- Use argparse for CLI entry points and keep scripts runnable from the command line.
- Preserve idempotency and deduplication patterns where relevant.
- Avoid introducing unnecessary dependencies or hard-coded environment assumptions.

## SQL conventions
- Keep SQL scripts self-contained and easy to follow.
- Use clear comments to explain business logic and transformation steps.
- Prefer readable CTEs and explicit aliases.
- Preserve the intended assessment semantics for summaries, ranking, and regional analysis.

## Testing expectations
- If you change behavior, add or update tests in tests/.
- Prefer pytest-style tests and keep assertions specific and meaningful.
- Validate changes with the relevant test command when possible.

## Documentation conventions
- Update docs in docs/ when architecture, logic, or design decisions change.
- Keep documentation concise, structured, and aligned with the repository’s assessment narrative.

## Safety and scope
- Do not add production secrets, credentials, or sensitive data.
- Do not change repository behavior unless the request clearly requires it.
- If a change could affect multiple parts of the solution, explain the impact briefly before making broad edits.
