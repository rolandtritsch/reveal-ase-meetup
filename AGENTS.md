# AGENTS.md — reveal-ase-meetup

## Project Overview

**reveal-ase-meetup** — Slide decks and supporting documents for the **Augmented Software Engineer (ASE) Meetup**.

- **Purpose**: Monthly meetup where software engineers exchange knowledge about leveraging AI in software development
- **Author**: Roland Tritsch
- **Format**: Content authored in **Emacs org-mode**, published with **reveal.js** via `org-re-reveal`
- **Output**: HTML slides, PDFs (slides + speaker notes), and documents published to `public/`
- **Meetup**: [The Augmented Software Engineer](https://www.meetup.com/the-augmented-software-engineer)

---

## Repository Structure

```
reveal-ase-meetup/
├── slides/              # Org-mode slide decks (*-slides.org)
│   ├── 000-000-kickoff-slides.org
│   ├── 001-000-kickoff-slides.org
│   ├── 002-010-warp-slides.org
│   ├── 002-020-ollama-slides.org
│   ├── 002-030-mcp-slides.org
│   ├── 003-000-kickoff-slides.org
│   ├── 004-000-kickoff-slides.org
│   ├── 005-000-kickoff-slides.org
│   ├── 006-000-kickoff-slides.org
│   ├── 007-000-kickoff-slides.org
│   ├── 008-000-kickoff-slides.org
│   ├── 999-000-practioner-slides.org
│   └── ... (wrapup, noslides variants)
├── docs/                # Supporting documents (*-doc.org)
│   ├── 000-desc-doc.org      # Overview/description
│   ├── 010-events-doc.org    # Past events
│   ├── 020-roles-doc.org     # Roles
│   └── 999-concept-doc.org   # Concepts
├── pdfs/                # External/reference PDFs (copied to public/)
├── images/              # Images used in slides
├── scripts/             # Build automation
│   ├── build-slides.sh     # Build slides to HTML/PDF
│   ├── build-doc.sh        # Build documents to PDF
│   └── print-slides.cjs    # Puppeteer script for PDF generation
├── public/              # Generated output (gitignored)
├── Makefile             # Build orchestration
└── README.md
```

---

## Prerequisites

| Tool | Purpose | Install |
|------|---------|---------|
| Emacs + org-mode | Authoring slides/docs | System package manager |
| `org-re-reveal` | Export org → reveal.js HTML | ELPA/MELPA |
| Node.js + Puppeteer | HTML → PDF (slides with notes) | `npm install -g puppeteer` |
| `pdflatex` | Document PDF generation | TeX Live / MacTeX |
| `make` | Build orchestration | System package manager |

---

## Common Commands

```bash
# Build everything → public/
make publish

# Clean generated files in public/
make clean

# Clean ALL generated artifacts (HTML/PDF/TeX) except pdfs/
make clean-full

# Build a single slide deck
./scripts/build-slides.sh slides/000-000-kickoff-slides.org

# Build a single document
./scripts/build-doc.sh docs/000-desc-doc.org
```

---

## Building Slides & Documents

### Slides (`scripts/build-slides.sh`)
1. Exports org → reveal.js HTML via `org-re-reveal`
2. Uses Puppeteer (`print-slides.cjs`) to generate:
   - `*-slides.pdf` — slides only
   - `*-slides_with-notes.pdf` — slides + speaker notes
3. Outputs to `public/`

### Documents (`scripts/build-doc.sh`)
1. Exports org → LaTeX → PDF via `pdflatex`
2. Outputs to `public/`

### Org-re-reveal Conventions
- **Headers** → Slides: `*` = horizontal, `**` = vertical
- **Properties**: `REVEAL_ROOT`, `REVEAL_THEME`, `REVEAL_TRANSITION`
- **Speaker notes**: `#+BEGIN_NOTES` / `#+END_NOTES` blocks
- **Images**: `[[file:../images/name.png]]` (relative to slides/)

---

## Adding New Content

### New Slide Deck
1. Create `slides/XXX-YYY-descriptive-slides.org`
2. Follow numbering convention: `XXX` = session, `YYY` = order
3. Add reveal.js header properties (see existing files)
4. Run `make publish`

### New Document
1. Create `docs/XXX-descriptive-doc.org`
2. Add LaTeX header options as needed
3. Run `make publish`

### New External PDF
1. Place in `pdfs/`
2. Will be copied to `public/` on `make publish`

---

## Coding Conventions

### Org-mode
- Use `#+TITLE`, `#+SUBTITLE`, `#+AUTHOR`, `#+DATE` headers
- Number files with 3-digit prefixes for ordering
- Speaker notes in `#+BEGIN_NOTES` blocks
- Images referenced relatively: `../images/`

### Slide Design
- Minimal text, visual focus
- Consistent theme (see `REVEAL_THEME` property)
- Use `#+ATTR_REVEAL: :frag roll-in` for animations
- Code blocks with `#+BEGIN_SRC language`

### Git
- Commit org source files, not generated output
- `public/` is gitignored (generated)
- `pdfs/` contains committed reference PDFs

---

## Key Files to Know

| File | Purpose |
|------|---------|
| `Makefile` | Build orchestration, targets |
| `scripts/build-slides.sh` | Slide build pipeline |
| `scripts/build-doc.sh` | Document build pipeline |
| `scripts/print-slides.cjs` | Puppeteer PDF generation |
| `docs/000-desc-doc.org` | Project overview |
| `docs/010-events-doc.org` | Past events log |

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Puppeteer fails | `npm install -g puppeteer` (downloads Chromium) |
| `pdflatex` missing | Install TeX Live / MacTeX |
| org-re-reveal not loading | `M-x package-install RET org-re-reveal` |
| Images not showing | Check relative paths from `slides/` to `../images/` |
| PDF generation hangs | Increase Puppeteer timeout in `print-slides.cjs` |

---

## References

- [org-re-reveal](https://github.com/yjwen/org-re-reveal) — Org → reveal.js exporter
- [reveal.js](https://revealjs.com/) — HTML presentation framework
- [ASE Meetup](https://www.meetup.com/the-augmented-software-engineer) — Meetup page
- [Augmented Software Engineer article](https://tedn.life/2024/10/24/the-augmented-software-engineer-what-is-it)