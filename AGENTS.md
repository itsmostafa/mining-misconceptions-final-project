# Repository Guidelines

## Project Structure & Module Organization
The repo stays lightweight: `pyproject.toml` and `uv.lock` define dependencies, while `bbc-category-project.ipynb` holds the exploratory modeling work. Keep production-ready code in a `src/` package (create it if absent) so reusable preprocessing, feature, and model utilities are importable both from notebooks and future CLIs. Place datasets under `data/raw/` (read-only) and `data/processed/` (generated artifacts) and never commit PII or large binaries because `.gitignore` already excludes checkpoints and virtual environments.

## Build, Test, and Development Commands
- `uv sync`: install and lock dependencies for Python 3.13 inside `.venv`.
- `uv run jupyter lab` (or `notebook`): launch the UI, then open `bbc-category-project.ipynb` for experiments.
- `uv run python -m pytest tests/`: execute unit tests once modules land in `src/`.
- `uv run python -m nltk.downloader punkt stopwords`: download tokenizers used by the notebook to avoid runtime prompts.

## Coding Style & Naming Conventions
Follow PEP 8 with 4-space indentation, line length ≤ 100, and type hints on public functions. Favor pandas column names in snake_case (`category_label`, `clean_text`) and sklearn pipelines in camel-case nouns (`ArticleClassifier`). Notebook cells should progress from data load → preprocessing → modeling; name checkpoints like `01_cleaning`, `02_modeling` to keep execution order obvious.

## Commit & Pull Request Guidelines
Commits are single-purpose and use imperative summaries like the existing "Initial commit"; include context in the body when touching data or models. Every PR should link the tracking issue, describe dataset versions, and attach before/after metrics or key plots (PNG exports under `artifacts/`). Request review when CI passes, tests are green, notebooks execute without warnings, and new dependencies are reflected in `pyproject.toml` + `uv.lock`.

## Environment & Data Handling
Pin Python via `.python-version` and regenerate environments with `uv sync --frozen` for reproducibility. Never commit raw BBC datasets; instead, document download steps in `README.md` and rely on `.gitignore` to keep local caches private. Store API keys or labeling credentials in `.env` (ignored) and surface required variable names in the PR description so other agents can reproduce runs safely.
