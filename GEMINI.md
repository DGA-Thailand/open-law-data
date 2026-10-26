# GEMINI.md — Open Law Data Thailand Web Portal (`open-law-data-web`)

> **Antigravity / Gemini Fast-Context Rules**
> Read [`AGENTS.md`](AGENTS.md) first for the complete **Tech Stack**, **Data Contracts**, and **Surgical Line-Number Index** of [`public/index.html`](public/index.html) (1,359 lines).

## Key Rules for This Workspace
1. **Language**: Always respond to the user in **Thai** (ภาษาไทย). Keep UI headers in `public/index.html` in Thai.
2. **Surgical File Reads**: Do **not** read the entire `public/index.html` at once. Use the line-number index in [`AGENTS.md §2`](AGENTS.md#2-surgical-line-number-index-for-publicindexhtml-1359-lines) with `StartLine` and `EndLine`.
3. **Zero-Build Static Architecture**: All web assets live in `public/` and deploy directly to GitHub Pages via `.github/workflows/static.yml`. Do not introduce npm build steps unless explicitly requested by the user.
4. **Local Preview**: Run `python3 -m http.server 8000 --directory public` to test changes locally (required for `fetch('./data/showcases.toml')`).
5. **Reference Docs**:
   - Architecture & Data Flow: [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)
   - Schemas & HF CLI Recipes: [`docs/DATA_DICTIONARY.md`](docs/DATA_DICTIONARY.md)
   - Developer Guide & Known Issues Audit: [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md)
   - Roadmap & Next Steps: [`docs/ROADMAP.md`](docs/ROADMAP.md)
