# AGENTS.md — Open Law Data Thailand Web Portal (`open-law-data-web`)

> **Token-Efficiency & Zero-Exploration Mandate**
> This file pre-loads the **Tech Stack**, **File Map**, **Data Contracts**, **Surgical Line-Number Index**, and **Development Rules** for `open-law-data-web` (`github.com/DGA-Thailand/open-law-data`) so AI coding agents can navigate and modify the repository immediately without reading the entire 1,227-line `public/index.html` cold.

---

## 0. Language & Communication Rules
- **User Communication**: Always reply to the user in **Thai** (ภาษาไทย) unless asked otherwise.
- **Website Content**: Primary UI copy in `public/index.html` is **Thai** (with concise English section badges). Keep section headers in Thai (see commit `e490b67`).
- **Commit Messages**: Concise English (e.g., `feat: ...`, `fix: ...`, `docs: ...`, `chore: ...`).

---

## 1. Fixed Tech Stack & Architecture
- **Architecture**: **100% Stateless Static Website** deployed directly from `public/` to **GitHub Pages** via [`.github/workflows/static.yml`](.github/workflows/static.yml).
- **Zero Build Step**: There is **no** `package.json`, bundler, or build step. Everything under `public/` is served as-is.
- **Frontend Dependencies (Loaded via CDN in `public/index.html`)**:
  - **Tailwind CSS v3 CDN**: `https://cdn.tailwindcss.com` with inline `tailwind.config` (`Prompt` font, `thai-blue: #1e3a8a`, `thai-dark: #0f172a`, `tech-accent: #3b82f6`).
  - **Phosphor Icons**: `https://unpkg.com/@phosphor-icons/web` (`ph-*`, `ph-fill`, `ph-bold`).
  - **fast-toml (`0.5.4`)**: `https://unpkg.com/fast-toml@0.5.4/fast-toml.js` bound to `window.toml` for client-side parsing of `public/data/showcases.toml`.
  - **Google Analytics**: `G-5MHZ1LPBH1`.

---

## 2. Surgical Line-Number Index for `public/index.html` (1,359 Lines)

Always use targeted line ranges (`StartLine` / `EndLine`) when reading `public/index.html`:

| Line Range | Section / Component | Key Elements & IDs |
| :--- | :--- | :--- |
| `L1–152` | `<head>` & Styles | Meta/OG tags, Google Fonts (`Prompt`), Favicons, GA4 (`G-5MHZ1LPBH1`), Tailwind CDN + config, Phosphor Icons, `fast-toml`, keyframe animations (`float`, `fade-in-up`) |
| `L156–200` | `<nav>` Navigation Bar | Desktop menu (`L165–172`), Mobile menu toggle & `#mobile-menu` (`L174–199`) |
| `L202–250` | Hero Section | `#latest-update-trigger`, `#update-indicator`, `#latest-update-text`, Main title, Hugging Face CTA button |
| `L252–294` | `#stats` Section | 4 metric cards: `2428` (oldest year), `1.34M+` (files), `170GB+` (size), `24 Hrs` (update cycle) |
| `L298–322` | `#showcase` Section | Community Showcase header & `#showcase-grid` dynamic container |
| `L324–436` | `#about` Section | Project Introduction: Background (`L336–356`), Objectives (`L358–388`), Cooperation Model (`L390–432`) |
| `L438–496` | `#architecture` Section | 3-step pipeline cards: 1. Data Aggregation, 2. Storage & OCR, 3. Public Access |
| `L498–562` | `#use-cases` Section | 3 use-case cards: Legal Research AI, Compliance Automation, Academic Analysis |
| `L566–635` | `#partners` Section | 4 partner cards: วุฒิสภา (`senate.jpg`), สลค. (`soc.jpg`), กฤษฎีกา (`ocs.png`), DGA (`dga.png`) |
| `L637–699` | `#credits` Section | Technology supporters: UpPass (`uppass.svg`) and iApp Technology (`iapp.svg`) |
| `L703–827` | `#data-preview` Section | Interactive tabs (`meta/` / `ocr/` / `hf cli`) + Copy button (`L719–765`) + Data Dictionary table & Supported Formats (`L767–824`) |
| `L829–845` | Bottom CTA Section | Secondary Hugging Face download CTA |
| `L848–887` | `<footer>` | Mission statement, HF/GitHub/Discord links, License notice (`L878`), Copyright (`L884`) |
| `L890–901` | `#update-popup` Modal | Root modal overlay container for live dataset status |
| `L903–1254` | Script 1: Preview Tabs & Live Status Poller | `escapeHtml()`, `safeUrl()`, `#data-preview` tab switcher, polls `sources[]` (`latest_update.json` + `coming_soon` support), updates `#latest-update-text` & `#update-popup` |
| `L1255–1356` | Script 2: Showcase Loader | `loadShowcases()` & `renderCards()` — fetches `./data/showcases.toml`, parses via `toml.parse()`, sorts (`featured` desc, `date` desc), safely renders `#showcase-grid` |

---

## 3. External & Internal Data Contracts

### 3.1 Live Pipeline Status (`latest_update.json` on Hugging Face)
Fetched client-side in `public/index.html#L951-L1113`:
- **Current Source URL**: `https://huggingface.co/datasets/open-law-data-thailand/soc-ratchakitcha/resolve/main/latest_update.json`
- **Expected JSON Shape**:
  ```json
  {
    "last_updated_pipeline": "2026-01-19T14:30:00Z",
    "latest_record": {
      "doctitle": "...",
      "publishDate": "2026-01-19",
      "pdf_file": "2026-000123.pdf"
    }
  }
  ```

### 3.2 Community Showcase (`public/data/showcases.toml`)
Fetched client-side in `public/index.html#L1137-L1223`:
- **Expected TOML Entry**:
  ```toml
  [[projects]]
  title = "..."
  author = "..."
  author_url = "https://..."
  description = "..."
  tags = ["Tag1", "Tag2"]
  url = "https://..."
  featured = false
  date = "YYYY-MM-DD"
  ```

---

## 4. Engineering Rules & Gotchas

1. **Local Testing Requires an HTTP Server**:
   - Because `fetch('./data/showcases.toml')` is used, opening `public/index.html` via `file://` fails with CORS. Always test with `python3 -m http.server 8000 --directory public`.
2. **Tailwind CDN Dynamic Class Gotcha**:
   - In `public/index.html#L1056-L1084`, popup cards interpolate `source.color` into class names (`bg-${source.color}-50`, `text-${source.color}-600`, etc.). When adding a new data source with a new color, avoid dynamic string concatenation if the full class name does not already exist in the HTML; prefer a complete class lookup map.
3. **Security / XSS Prevention in `showcases.toml`**:
   - `renderCards()` (`L1174–1206`) injects fields from `showcases.toml` into `innerHTML`. Never allow `javascript:` URLs or unescaped HTML tags in `public/data/showcases.toml`.
4. **Known Technical Backlog**:
   - Check [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md#4-รายการตรวจสอบทางเทคนิคและจุดที่ควรปรับปรุง-technical-audit--known-issues) and [`docs/ROADMAP.md`](docs/ROADMAP.md) for documented minor issues and planned enhancements before starting new features.

---

## 5. Documentation Map
- [`README.md`](README.md) — Project overview, dataset quickstart (`hf download`), repo structure
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — Showcase PR guide & web contribution rules
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — End-to-end NAS -> Hugging Face -> GitHub Pages architecture
- [`docs/DATA_DICTIONARY.md`](docs/DATA_DICTIONARY.md) — Schemas for HF datasets, `latest_update.json`, and `showcases.toml`
- [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md) — Local dev server, UI tokens, recipes, and technical audit list
- [`docs/ROADMAP.md`](docs/ROADMAP.md) — Project phases and upcoming tasks
