# Repository Guidelines

## Project Structure & Module Organization
- Source: `creating_a_training_module/creating_a_training_module/` (Markdown/MyST pages, notebooks, config).
- Build output: `creating_a_training_module/creating_a_training_module/_build/html/`.
- Repo docs: `creating_a_training_module/README.md`, `CONTRIBUTING.md`, `CONDUCT.md`.
- Dependencies: `creating_a_training_module/requirements.txt`.
- Assets: keep images in the book folder (e.g., `logo.png`, `hsf-logo.png`). Update `_toc.yml` when adding pages.

## Build, Test, and Development Commands
- Install deps: `pip install -r creating_a_training_module/requirements.txt` (use a virtual env).
- Clean build: `jupyter-book clean creating_a_training_module/creating_a_training_module/`.
- Build site: `jupyter-book build creating_a_training_module/creating_a_training_module/`.
- Preview locally: `python -m http.server -d creating_a_training_module/creating_a_training_module/_build/html 8000` then open `http://localhost:8000`.
- Deploy (GitHub Pages flow): `ghp-import -n -p -f creating_a_training_module/creating_a_training_module/_build/html`.

## Coding Style & Naming Conventions
- Content: prefer MyST Markdown (`.md`) with Jupyter Book syntax; notebooks (`.ipynb`) only when execution is required.
- File names: lowercase-with-dashes (e.g., `markdown-notebooks.md`).
- Structure: register new pages in `_toc.yml`; keep one topic per page.
- Formatting: wrap lines at ~100 chars; use fenced code blocks with language hints (e.g., ```bash).

## Testing Guidelines
- Validate build strictly: `jupyter-book build -W --keep-going creating_a_training_module/creating_a_training_module/` (treat warnings as errors).
- Link checks (optional): `jupyter-book build --builder linkcheck creating_a_training_module/creating_a_training_module/`.
- Notebook execution: keep run times short; pin random seeds; prefer static outputs where possible.

## Commit & Pull Request Guidelines
- Commits: use Conventional Commits (e.g., `docs: add intro section`, `fix: correct broken link`).
- PRs: include a clear description, screenshots of rendered pages (if UI/content changes), and link related issues.
- Scope: keep PRs small and focused; update `_toc.yml` and assets paths when pages move.

## Security & Configuration Tips
- Use a virtual environment (`python -m venv venv && source venv/bin/activate`).
- Record environment for reproducibility (e.g., `pip freeze > requirements.lock`).
- Avoid embedding secrets in notebooks; clear outputs that contain tokens or personal data.
