# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Hugo static site for **mmetrics.ai** (Moustache Metrics) — a boutique AI operations consulting practice in Sydney. Uses the **Terminal** theme as a starting point, to be customized with brand assets.

**Owner:** Geoff Pidcock — ex-Atlassian, ex-Salesforce, MBA. Currently focused on AI safety through work in Nuclear.

**Goal:** Demonstrate AI proficiency through working demos, not just describe it. Show don't tell.

## Commands
```bash
hugo serve                              # Local dev server at http://localhost:1313
hugo                                    # Build static site to /public/
hugo new posts/my-post.md               # Create new blog post from archetype
git submodule update --init --recursive # Init theme if cloned without --recurse-submodules
```

No test, lint, or CI commands — pure Hugo content site.

## Architecture

- **config.toml** — Site config (renamed from `hugo.toml` for version compatibility)
- **content/** — Markdown with YAML front matter. `posts/` for blog, `about.md`, `contact.md`
- **layouts/partials/** — Theme overrides (don't edit `themes/terminal/` directly)
- **themes/terminal/** — Git submodule, do not edit
- **static/** — Logo files, favicon, custom assets

## Brand & Positioning

### Identity
- **Not:** Consultant selling strategy decks
- **Am:** Fixer who makes things work
- **Entry question:** "What's the spreadsheet everyone hates?"

### Target Audience
SME owners and ops leaders drowning in spreadsheets, wasting money on AI tools nobody uses, or stuck figuring out where to start.

### Tone
- Warm, approachable, slightly irreverent
- Fun but credible — personality that cuts through generic consultant sites
- Not corporate, not try-hard startup
- Tagline: "A boutique practice exploring the intersection of AI, safety, business, and facial hair ~.~"

### Language Rules (IMPORTANT)
Never say → Say instead:
- RevOps → Sales and ops processes
- Digital transformation → Getting things automated
- AI-enabled operations → AI that actually works
- Proof of concept → A pilot to test if this works
- Operationalize → Get it live / make it stick
- North star metrics → The numbers that matter

### Services (for reference in content)
| Service | Price | Timeline | Description |
|---------|-------|----------|-------------|
| The Quick Win | $5-10K | 2-3 weeks | One painful process → working fix → live |
| The Pilot | $15-25K | 4-6 weeks | Test if AI solves a bigger problem |
| The Fix | $30-50K | 6-10 weeks | End-to-end workflow automation |

## Design Direction

### Brand Colors (from logo work)
- **Primary:** Blue and Orange — dual mustache motif
- **Mode:** Dark background with light text
- White logo variant for dark backgrounds, black for light

### Current State
- Terminal theme is cookie-cutter placeholder
- Needs customization to incorporate:
  - Moustache Metrics logo (blue/orange dual mustache)
  - Consistent color palette from logo
  - Custom favicon
  - Brand fonts (TBD)

### Assets Needed
- [ ] Logo files (SVG preferred) for `static/` folder
- [ ] Favicon set
- [ ] Color hex codes finalized from logo

## Planned Features

- **llms.txt** — AI discoverability for Perplexity/ChatGPT/Gemini crawlers
- **PyScript** — Interactive Python demos in blog posts (browser-based)
- **GitHub Actions** — Automated deployment to GitHub Pages

## Development Notes

- Geoff works primarily on tablet, prefers iterative "baby steps" development
- `markup.goldmark.renderer.unsafe = true` — raw HTML allowed in markdown
- Menu has 3 items; `showMenuItems = 3` must match actual count
- Config uses TOML format for broad Hugo version compatibility