# Design: Static HTML Blog + total-recall Post

## Overview

A static HTML blog hosted on GitHub Pages at `strvmarv.github.io/blog/` (with custom domain `blog.paulmarv.in`). First post is a problem/solution technical analysis of [total-recall](https://github.com/strvmarv/total-recall), a multi-tiered memory and knowledge base plugin for TUI coding assistants.

## Target Audience

1. Developer-tool builders (primary) — people who might build similar plugins
2. AI coding assistant users — people using Claude Code, Copilot CLI, etc. who want better memory
3. General technical audience — devs curious about the "AI tools forget everything" problem

## Architecture

Pure static HTML. No build step, no dependencies, no static site generator.

### File Structure

```
blog/
├── index.html                              # Post listing page
├── style.css                               # Shared styles (light/dark, typography, TUI)
├── theme.js                                # Theme toggle (~20 lines, localStorage)
├── CNAME                                   # blog.paulmarv.in
├── posts/
│   └── 2026-04-05-total-recall/
│       └── index.html                      # First blog post
└── .nojekyll                               # Bypass Jekyll processing
```

### URL Scheme

- Listing: `blog.paulmarv.in/`
- Posts: `blog.paulmarv.in/posts/2026-04-05-total-recall/`

## Visual Design

### Style

Option B — clean minimal. Prioritizes readability and lets content breathe.

- **Body font:** Inter (400/500/600/700)
- **Code/TUI font:** JetBrains Mono (400/500)
- **Content max-width:** 680px, centered
- **Theme:** Light default, dark mode via CSS custom properties
- **Toggle:** Button in header, persists to `localStorage`

### Color Tokens (CSS Custom Properties)

**Light mode:**
- `--bg: #fafafa`
- `--bg-surface: #ffffff`
- `--text: #1a1a1a`
- `--text-secondary: #666`
- `--border: #e5e5e5`
- `--accent: #2563eb`
- `--code-bg: #f3f4f6`

**Dark mode:**
- `--bg: #111113`
- `--bg-surface: #1a1a1e`
- `--text: #e4e4e7`
- `--text-secondary: #a1a1aa`
- `--border: #27272a`
- `--accent: #60a5fa`
- `--code-bg: #27272a`

**TUI blocks (both themes):**
- `--tui-bg: #1e1e2e` (light) / `#0a0a0c` (dark)
- Prompt: `#a6e3a1`, Info: `#89b4fa`, Warn: `#f9e2af`, Bright: `#cdd6f4`, Dim: `#585b70`

### Layout Components

- **Header:** `strvmarv` logo (left), `Posts | About | GitHub` nav + theme toggle (right)
- **Footer:** Simple one-liner with links
- **Tags:** Pill-shaped badges (`memory`, `mcp`, `sqlite`, `vector-search`)
- **Tier cards:** Three-column flex layout (Hot/Warm/Cold) with colored headers

## TUI Mockups

Two fidelity levels:

### Realistic (before/after hero mockups)

Match Claude Code's actual TUI chrome: box-drawing characters (`╭╮╰╯│─`), status line, prompt styling. Used for:
- "Problem" blank-slate session
- "Solution" total-recall-loaded session

### Stylized (architecture/data flow)

Simplified boxes showing structure and flow. Clarity over fidelity. Used for:
- Tier architecture diagram
- Data flow (store → embed → persist → search)
- Platform convergence diagram
- Eval report output

### Technical approach

- `<pre>` elements with `white-space: pre`
- JetBrains Mono font
- Dark background regardless of page theme
- Colored `<span>` elements for syntax highlighting
- Careful character-counting for box-drawing alignment

## Blog Post: "Your AI Coding Assistant Forgets Everything"

### Section 1: The Problem

AI coding assistants forget everything between sessions. Built-in memory systems are flat files with no tiering, no semantic search, tool-locked, and no observability.

**TUI mockup (realistic):** A fresh Claude Code session with warnings — no memory of previous sessions, no project context, previous corrections forgotten. User asks to refactor auth middleware; assistant repeats the exact mistake corrected last session.

### Section 2: The Solution

total-recall introduces a three-tier memory model:
- **Hot** (<=50 entries): auto-injected into every prompt
- **Warm** (<=10K entries): retrieved semantically per query
- **Cold** (unlimited): hierarchical knowledge base

**TUI mockup (realistic):** Same session but with total-recall loaded — shows tier counts, last session time, injected context (auth compliance, real DB for tests, bundled PRs preference). Assistant now remembers and acts on the corrections.

**Visual element:** Three tier cards (HTML/CSS, not ASCII) showing limits and behavior.

### Section 3: Architecture Deep Dive

- SQLite + sqlite-vec for storage and vector search
- all-MiniLM-L6-v2 embeddings via ONNX (384 dimensions, runs locally)
- Compaction with configurable decay half-life
- Data flow: store → embed → vector index → search → rank

**Stylized diagram:** Data flow pipeline showing how a memory moves from write to retrieval.

### Section 4: Cross-Platform Story

- MCP (Model Context Protocol) as universal interface
- Host importers: Claude Code, Copilot CLI, Cursor, Cline, OpenCode, Hermes
- Existing memories migrate automatically on first run
- One memory store works across all tools

**Stylized diagram:** Multiple platform icons converging into a single total-recall database.

### Section 5: Observability

- Eval framework with 139-query benchmark suite
- Retrieval quality metrics (7-day rolling)
- Config snapshots for A/B comparison
- Regression detection on session start
- Benchmark candidate capture from retrieval misses (eval grows itself)

**Stylized TUI mockup:** Eval report output showing precision, recall, and comparison between configs.

### Section 6: Getting Started

- One-line install for Claude Code: `/plugin install total-recall@strvmarv-total-recall-marketplace`
- npm global install for other MCP tools: `npm install -g @strvmarv/total-recall`
- Self-install via any AI coding assistant (paste instruction URL)
- Link to GitHub repo and npm package

## GitHub Pages Setup

- Enable Pages in repo settings (deploy from `main` branch, root `/`)
- `.nojekyll` file to bypass Jekyll processing (preserves files starting with `_`)
- `CNAME` file with `blog.paulmarv.in`
- User configures DNS: CNAME record `blog.paulmarv.in` → `strvmarv.github.io`
- No GitHub Actions workflow needed — Pages deploys directly from branch

## Non-Goals

- No RSS feed (can add later)
- No comments system
- No analytics
- No build step or CI pipeline (unless needed for Pages deploy)
- No JavaScript framework
