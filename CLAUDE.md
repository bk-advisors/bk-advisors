# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BK Advisors is a static website for a management consulting firm based in Kampala, Uganda. The site is hosted on ecowebhosting (FTP) and mirrored via GitHub Pages at `bk-advisors.github.io`.

## Architecture

The project has two distinct parts:

1. **Main website** (root level) — Static HTML/CSS/JS pages. No build system, bundler, or package manager. All files are directly uploadable via FTP.
   - `index.html` — Single-page landing site with sections: Home (hero), About, Services, Blog previews, Clients/Experience, Contact
   - `blog.html` — Dedicated blog listing page with category tab filtering (Bootstrap pills)
   - `brochure.html` — Print-ready A4 landscape tri-fold brochure (CSS Grid, `@page` for print, self-contained styles)
   - `styles.css` — Shared stylesheet with CSS custom properties design system

2. **Blog/Quarto project** (`blog/`) — A Quarto website project for data-driven blog posts using R.
   - `_quarto.yml` — Quarto config; outputs to `blog-posts/` directory, uses cosmo theme
   - `blog-template.qmd` — Template for new blog posts (R code chunks with tidyverse)
   - `R-scripts/` — Standalone R scripts (data acquisition, visualization, helper functions)
   - `_freeze/` — Quarto freeze cache for computed outputs (`execute: freeze: auto`)

## CDN Dependencies

All external resources are loaded via CDN (no local copies, no npm):
- **Bootstrap 5.3** — CSS + JS (including Popper)
- **Bootstrap Icons** — icon font
- **Google Fonts** — Inter (body text) and Poppins (headings)
- **AOS.js 2.3.4** — Animate on Scroll library (CSS + JS from unpkg)

## Design System (`styles.css`)

The stylesheet uses CSS custom properties (`:root` variables) for the entire design:
- **Color palette**: `--color-primary` (navy #1a1a4e), `--color-accent` (teal #0ea5a0), plus light variants
- **Typography**: `--font-heading` (Poppins), `--font-body` (Inter)
- **Effects**: Glassmorphism variables (`--glass-bg`, `--glass-blur`), shadow scale (`--shadow-sm/md/lg/hover`), border radius tokens
- **Components**: `.section-title` (with gradient underline `::after`), `.service-card`, `.blog-card`, `.contact-card`, `.footer`, `.btn-hero`

The navbar uses a transparent-to-frosted-glass scroll effect controlled by a `.scrolled` class toggled via inline JS in each HTML page.

## Development

- **Main site**: Open `index.html` directly in a browser or use any local server (e.g., `python -m http.server`). No install step needed.
- **Blog posts**: Requires R and Quarto. Render with `quarto render` from the `blog/` directory. Output goes to `blog/blog-posts/`.
- **Deployment**: Push to `main` branch triggers GitHub Actions workflow (`.github/workflows/deploy.yml`) which auto-FTPs changed files to ecowebhosting. GitHub Pages also serves from the repository root.

## Deployment (FTP via GitHub Actions)

The workflow at `.github/workflows/deploy.yml` uses `SamKirkland/FTP-Deploy-Action@v4.3.5`. It requires three repository secrets: `FTP_SERVER`, `FTP_USERNAME`, `FTP_PASSWORD`. The `server-dir` is set to `/public_html/`. The `blog/` directory, R project files, and dev docs are excluded from FTP upload.

## Key Conventions

- Services and blog sections use responsive grid (`row g-4` with `col-md-6 col-lg-4`), not horizontal scroll
- Client logos use grayscale filter with hover-to-color transition
- Image assets are duplicated in both `assets/` (root) and `blog/assets/`
- The `.gitignore` excludes R project files (`.Rproj.user`, `.Rhistory`, `.RData`, `.Ruserdata`)
- `brochure.html` is self-contained (inline styles) — do not link it to `styles.css`
