# CLAUDE.md — Open Law Data Thailand Web Portal (`open-law-data-web`)

See **[`AGENTS.md`](./AGENTS.md)** for the complete orientation, **Surgical Line-Number Index** for `public/index.html`, data contracts (`latest_update.json` and `public/data/showcases.toml`), and engineering rules.

## Quick Commands

```bash
# Serve the static website locally (required so fetch('./data/showcases.toml') works without CORS errors)
python3 -m http.server 8000 --directory public
```

## Documentation Index
- **[`AGENTS.md`](./AGENTS.md)** — Master AI agent guide & `public/index.html` line-number map
- **[`docs/ARCHITECTURE.md`](./docs/ARCHITECTURE.md)** — End-to-end NAS -> Hugging Face -> Stateless Web Portal architecture
- **[`docs/DATA_DICTIONARY.md`](./docs/DATA_DICTIONARY.md)** — Dataset directory structure, JSON/TOML schemas, and `hf download` recipes
- **[`docs/DEVELOPMENT.md`](./docs/DEVELOPMENT.md)** — Design tokens, how to onboard new data sources, and technical audit/backlog
- **[`docs/ROADMAP.md`](./docs/ROADMAP.md)** — Upcoming milestones (OCS integration, Showcase enhancements, Hackathon)
