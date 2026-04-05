# Static HTML Blog + total-recall Post — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a static HTML blog at strvmarv.github.io/blog/ with a first post analyzing total-recall's problem/solution architecture, including realistic and stylized TUI mockups.

**Architecture:** Pure static HTML/CSS — no build step, no dependencies. Light/dark theme via CSS custom properties + localStorage. Each post is an index.html in a dated folder. GitHub Pages serves directly from main branch.

**Tech Stack:** HTML5, CSS3 (custom properties, flexbox), vanilla JS (theme toggle only), Google Fonts (Inter, JetBrains Mono)

---

## File Structure

```
blog/
├── index.html                              # Post listing page
├── style.css                               # Shared styles: light/dark tokens, typography, TUI blocks, layout
├── theme.js                                # Theme toggle logic (~20 lines)
├── CNAME                                   # blog.paulmarv.in
├── .nojekyll                               # Bypass Jekyll processing
├── .gitignore                              # Already exists
├── docs/                                   # Specs and plans (not served)
└── posts/
    └── 2026-04-05-total-recall/
        └── index.html                      # Blog post
```

---

### Task 1: Repository Setup

**Files:**
- Create: `CNAME`
- Create: `.nojekyll`
- Modify: `.gitignore` (already exists, verify contents)

- [ ] **Step 1: Initialize git repo**

```bash
cd ~/source/blog
git init
git remote add origin git@github.com:strvmarv/blog.git
```

- [ ] **Step 2: Create CNAME file**

Create `CNAME` with contents:
```
blog.paulmarv.in
```

- [ ] **Step 3: Create .nojekyll file**

Create `.nojekyll` as an empty file (no contents needed).

- [ ] **Step 4: Verify .gitignore**

`.gitignore` should contain:
```
docs/superpowers/
.brainstorm/
.superpowers/
.remember/
```

Add `.remember/` if not already present.

- [ ] **Step 5: Commit**

```bash
git add CNAME .nojekyll .gitignore
git commit -m "chore: initialize blog repo with GitHub Pages config"
```

---

### Task 2: Shared Stylesheet (style.css)

**Files:**
- Create: `style.css`

This file defines ALL visual styles for the entire blog — light/dark tokens, typography, layout, TUI blocks, tier cards, tags, and responsive behavior.

- [ ] **Step 1: Create style.css with CSS custom properties and base typography**

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap');

*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

:root {
  --bg: #fafafa;
  --bg-surface: #ffffff;
  --text: #1a1a1a;
  --text-secondary: #666;
  --border: #e5e5e5;
  --accent: #2563eb;
  --code-bg: #f3f4f6;
  --tui-bg: #1e1e2e;
  --tui-border: #313244;
  --tui-text: #cdd6f4;
}

[data-theme="dark"] {
  --bg: #111113;
  --bg-surface: #1a1a1e;
  --text: #e4e4e7;
  --text-secondary: #a1a1aa;
  --border: #27272a;
  --accent: #60a5fa;
  --code-bg: #27272a;
  --tui-bg: #0a0a0c;
  --tui-border: #27272a;
}

body {
  background: var(--bg);
  color: var(--text);
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  font-size: 16px;
  line-height: 1.75;
  transition: background 0.2s, color 0.2s;
}
```

- [ ] **Step 2: Add header, nav, footer, and container layout styles**

Append to `style.css`:

```css
/* Layout */
.container { max-width: 680px; margin: 0 auto; padding: 1rem 1.5rem 4rem; }

/* Header */
header {
  max-width: 680px;
  margin: 0 auto;
  padding: 2rem 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-weight: 700;
  font-size: 1.1rem;
  color: var(--text);
  text-decoration: none;
}

nav { display: flex; align-items: center; gap: 0.25rem; }

nav a {
  color: var(--text-secondary);
  text-decoration: none;
  margin-left: 1.5rem;
  font-size: 0.9rem;
  font-weight: 500;
}

nav a:hover { color: var(--accent); }

.theme-toggle {
  background: var(--bg-surface);
  border: 1px solid var(--border);
  color: var(--text-secondary);
  padding: 4px 10px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.8rem;
  font-family: inherit;
  margin-left: 1.5rem;
  transition: border-color 0.2s;
}

.theme-toggle:hover { border-color: var(--accent); color: var(--accent); }

/* Footer */
footer {
  max-width: 680px;
  margin: 0 auto;
  border-top: 1px solid var(--border);
  padding: 2rem 1.5rem;
  text-align: center;
  color: var(--text-secondary);
  font-size: 0.85rem;
}

footer a { color: var(--text-secondary); text-decoration: none; }
footer a:hover { color: var(--accent); }
```

- [ ] **Step 3: Add typography and content styles**

Append to `style.css`:

```css
/* Typography */
h1 {
  font-size: 2rem;
  font-weight: 700;
  line-height: 1.25;
  margin-bottom: 0.75rem;
  letter-spacing: -0.02em;
  color: var(--text);
}

h2 {
  font-size: 1.35rem;
  font-weight: 600;
  margin: 2.5rem 0 1rem;
  letter-spacing: -0.01em;
  color: var(--text);
}

h3 {
  font-size: 1.1rem;
  font-weight: 600;
  margin: 1.5rem 0 0.75rem;
  color: var(--text);
}

p { margin-bottom: 1.25rem; color: var(--text-secondary); }

a { color: var(--accent); text-decoration: none; }
a:hover { text-decoration: underline; }

.subtitle {
  color: var(--text-secondary);
  font-size: 1.1rem;
  line-height: 1.6;
  margin-bottom: 2.5rem;
  padding-bottom: 2rem;
  border-bottom: 1px solid var(--border);
}

blockquote {
  border-left: 3px solid var(--accent);
  padding-left: 1.25rem;
  color: var(--text-secondary);
  margin: 1.5rem 0;
  font-style: italic;
}

code {
  background: var(--code-bg);
  color: var(--accent);
  padding: 2px 6px;
  border-radius: 4px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.85em;
}

ul, ol { margin: 0 0 1.25rem 1.5rem; color: var(--text-secondary); }
li { margin-bottom: 0.5rem; }
```

- [ ] **Step 4: Add post meta and tag styles**

Append to `style.css`:

```css
/* Post meta */
.post-meta {
  color: var(--text-secondary);
  font-size: 0.875rem;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex-wrap: wrap;
}

.tag {
  background: var(--code-bg);
  color: var(--text-secondary);
  padding: 2px 10px;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: 500;
}
```

- [ ] **Step 5: Add TUI block styles**

Append to `style.css`:

```css
/* TUI blocks */
.tui-block {
  background: var(--tui-bg);
  border: 1px solid var(--tui-border);
  border-radius: 8px;
  padding: 1.5rem;
  margin: 1.5rem 0;
  overflow-x: auto;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.8rem;
  line-height: 1.6;
  color: var(--tui-text);
  white-space: pre;
}

.tui-block .prompt { color: #a6e3a1; }
.tui-block .dim    { color: #585b70; }
.tui-block .info   { color: #89b4fa; }
.tui-block .warn   { color: #f9e2af; }
.tui-block .bright  { color: #cdd6f4; }
.tui-block .err    { color: #f38ba8; }
.tui-block .accent { color: #cba6f7; }
```

- [ ] **Step 6: Add tier card styles**

Append to `style.css`:

```css
/* Tier cards */
.tier-diagram {
  display: flex;
  gap: 1.25rem;
  margin: 2rem 0;
}

.tier-card {
  flex: 1;
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 1.25rem;
  background: var(--bg-surface);
}

.tier-card h3 {
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: 0.5rem;
  font-weight: 600;
}

.tier-card.hot h3  { color: #dc2626; }
.tier-card.warm h3 { color: #d97706; }
.tier-card.cold h3 { color: #2563eb; }

[data-theme="dark"] .tier-card.hot h3  { color: #f87171; }
[data-theme="dark"] .tier-card.warm h3 { color: #fbbf24; }
[data-theme="dark"] .tier-card.cold h3 { color: #60a5fa; }

.tier-card .limit {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 0.25rem;
}

.tier-card p {
  color: var(--text-secondary);
  font-size: 0.85rem;
  margin: 0;
  line-height: 1.5;
}
```

- [ ] **Step 7: Add post listing card styles (for index.html)**

Append to `style.css`:

```css
/* Post listing */
.post-list { list-style: none; padding: 0; margin: 2rem 0 0; }

.post-item {
  border-bottom: 1px solid var(--border);
  padding: 1.5rem 0;
}

.post-item:first-child { border-top: 1px solid var(--border); }

.post-item a {
  text-decoration: none;
  color: var(--text);
  font-size: 1.2rem;
  font-weight: 600;
  display: block;
  margin-bottom: 0.5rem;
}

.post-item a:hover { color: var(--accent); }

.post-item .excerpt {
  color: var(--text-secondary);
  font-size: 0.95rem;
  line-height: 1.6;
  margin-bottom: 0.5rem;
}

.post-item .post-meta { margin-bottom: 0; }
```

- [ ] **Step 8: Add responsive and flow diagram styles**

Append to `style.css`:

```css
/* Flow diagrams (stylized TUI) */
.flow-diagram {
  background: var(--tui-bg);
  border: 1px solid var(--tui-border);
  border-radius: 8px;
  padding: 2rem;
  margin: 1.5rem 0;
  overflow-x: auto;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.75rem;
  line-height: 1.5;
  color: var(--tui-text);
  white-space: pre;
  text-align: center;
}

/* Platform grid */
.platform-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0.75rem;
  margin: 1.5rem 0;
}

.platform-badge {
  background: var(--bg-surface);
  border: 1px solid var(--border);
  border-radius: 6px;
  padding: 0.75rem;
  text-align: center;
  font-size: 0.85rem;
  font-weight: 500;
  color: var(--text);
}

/* Responsive */
@media (max-width: 640px) {
  h1 { font-size: 1.5rem; }
  .tier-diagram { flex-direction: column; }
  .platform-grid { grid-template-columns: repeat(2, 1fr); }
  header { flex-wrap: wrap; gap: 1rem; }
  .tui-block, .flow-diagram { font-size: 0.7rem; padding: 1rem; }
}
```

- [ ] **Step 9: Open in browser and verify light mode renders correctly**

Open `style.css` isn't viewable alone — verify in Task 4 when index.html exists.

- [ ] **Step 10: Commit**

```bash
git add style.css
git commit -m "feat: add shared stylesheet with light/dark theme tokens"
```

---

### Task 3: Theme Toggle (theme.js)

**Files:**
- Create: `theme.js`

- [ ] **Step 1: Create theme.js**

```javascript
(function () {
  var stored = localStorage.getItem('theme');
  var prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  var theme = stored || (prefersDark ? 'dark' : 'light');
  document.documentElement.setAttribute('data-theme', theme);

  document.addEventListener('DOMContentLoaded', function () {
    var btn = document.querySelector('.theme-toggle');
    if (!btn) return;
    btn.textContent = theme === 'dark' ? 'light' : 'dark';
    btn.addEventListener('click', function () {
      theme = theme === 'dark' ? 'light' : 'dark';
      document.documentElement.setAttribute('data-theme', theme);
      localStorage.setItem('theme', theme);
      btn.textContent = theme === 'dark' ? 'light' : 'dark';
    });
  });
})();
```

- [ ] **Step 2: Commit**

```bash
git add theme.js
git commit -m "feat: add theme toggle with localStorage persistence"
```

---

### Task 4: Index Page (index.html)

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>strvmarv</title>
  <meta name="description" content="Technical writing on developer tools, AI coding assistants, and systems design.">
  <link rel="stylesheet" href="style.css">
  <script src="theme.js"></script>
</head>
<body>

<header>
  <a href="/blog/" class="logo">strvmarv</a>
  <nav>
    <a href="/blog/">Posts</a>
    <a href="https://github.com/strvmarv">GitHub</a>
    <button class="theme-toggle">dark</button>
  </nav>
</header>

<div class="container">
  <h1>Posts</h1>

  <ul class="post-list">
    <li class="post-item">
      <a href="posts/2026-04-05-total-recall/">Your AI Coding Assistant Forgets Everything. Here's How I Fixed It.</a>
      <p class="excerpt">A problem/solution analysis of total-recall — multi-tiered persistent memory for TUI coding assistants, backed by local SQLite + vector embeddings.</p>
      <div class="post-meta">
        <span>April 5, 2026</span>
        <span class="tag">memory</span>
        <span class="tag">mcp</span>
        <span class="tag">sqlite</span>
      </div>
    </li>
  </ul>
</div>

<footer>
  <a href="https://github.com/strvmarv">strvmarv</a> · built with plain html
</footer>

</body>
</html>
```

- [ ] **Step 2: Open in browser, verify light/dark toggle works**

```bash
cd ~/source/blog && python3 -m http.server 8787
```

Open `http://localhost:8787/` — verify:
- Header layout (logo left, nav + toggle right)
- Post listing with title, excerpt, tags
- Theme toggle switches colors
- Footer renders

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add index page with post listing"
```

---

### Task 5: Blog Post — Sections 1-2 (Problem + Solution)

**Files:**
- Create: `posts/2026-04-05-total-recall/index.html`

These sections contain the two realistic TUI mockups (before/after) and the tier cards. This is the visual centerpiece of the post.

- [ ] **Step 1: Create post directory**

```bash
mkdir -p ~/source/blog/posts/2026-04-05-total-recall
```

- [ ] **Step 2: Create the post HTML with head, header, and Section 1 (The Problem)**

Create `posts/2026-04-05-total-recall/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Your AI Coding Assistant Forgets Everything — strvmarv</title>
  <meta name="description" content="A problem/solution analysis of total-recall — multi-tiered persistent memory for TUI coding assistants.">
  <link rel="stylesheet" href="../../style.css">
  <script src="../../theme.js"></script>
</head>
<body>

<header>
  <a href="/blog/" class="logo">strvmarv</a>
  <nav>
    <a href="/blog/">Posts</a>
    <a href="https://github.com/strvmarv">GitHub</a>
    <button class="theme-toggle">dark</button>
  </nav>
</header>

<div class="container">
  <div class="post-meta">
    <span>April 5, 2026</span>
    <span class="tag">memory</span>
    <span class="tag">mcp</span>
    <span class="tag">sqlite</span>
    <span class="tag">vector-search</span>
  </div>

  <h1>Your AI Coding Assistant Forgets Everything. Here's How I Fixed It.</h1>
  <p class="subtitle">A problem/solution analysis of <a href="https://github.com/strvmarv/total-recall">total-recall</a> — multi-tiered persistent memory for TUI coding assistants.</p>

  <h2>The Problem</h2>

  <p>Every TUI coding assistant has the same gap. You spend an hour teaching Claude Code about your project's quirks — that the auth middleware was rewritten for compliance, that integration tests must hit a real database, that your team prefers bundled PRs for refactors — and then the session ends.</p>

  <p>Next session: blank slate.</p>
```

The realistic "before" TUI mockup follows. This must use `<pre class="tui-block">` with carefully character-counted box-drawing. Each line inside the box must be exactly 43 characters wide (between the left `│` and right `│`).

The box width is set by the top border: `╭` + 43 `─` chars + `╮` = 45 total. Every content line is `│` + 43 chars + `│` = 45 total.

```html
<pre class="tui-block"><span class="prompt">$</span> claude

<span class="dim">╭───────────────────────────────────────────╮</span>
<span class="dim">│</span><span class="bright"> Claude Code v1.0.23                       </span><span class="dim">│</span>
<span class="dim">│</span><span class="info"> Session started.                          </span><span class="dim">│</span>
<span class="dim">│</span>                                             <span class="dim">│</span>
<span class="dim">│</span><span class="warn"> ! No memory of previous sessions          </span><span class="dim">│</span>
<span class="dim">│</span><span class="warn"> ! No project context loaded               </span><span class="dim">│</span>
<span class="dim">│</span><span class="warn"> ! Previous corrections forgotten           </span><span class="dim">│</span>
<span class="dim">╰───────────────────────────────────────────╯</span>

<span class="prompt">&gt;</span> <span class="bright">refactor the auth middleware</span>

<span class="dim">Sure, I'll start by examining the auth middleware...</span>
<span class="dim">(proceeds to mock the database in tests)</span>
<span class="dim">(opens 3 separate PRs for a single refactor)</span>
<span class="dim">(ignores the compliance requirements entirely)</span></pre>
```

Then the prose bridge:

```html
  <p>The built-in memory systems are flat files. No tiering — every memory is treated equally, leading to context bloat or information loss. No semantic search — retrieval is by filename, not meaning. Switch from Claude Code to Copilot CLI? Start from scratch. No observability to know if memory is even helping.</p>
```

- [ ] **Step 3: Add Section 2 (The Solution) with tier cards and realistic "after" TUI mockup**

The "after" mockup box is wider: `╭` + 54 `─` chars + `╮` = 56 total. Every content line is `│` + 54 chars + `│` = 56 total.

```html
  <h2>The Solution</h2>

  <p><a href="https://github.com/strvmarv/total-recall">total-recall</a> introduces a three-tier memory architecture backed by local SQLite and vector embeddings. Memories aren't flat files — they're ranked, decayed, promoted, and searched semantically.</p>

  <div class="tier-diagram">
    <div class="tier-card hot">
      <h3>Hot</h3>
      <div class="limit">&le; 50</div>
      <p>Auto-injected into every prompt. Your most critical context, always present. Token-budgeted to 4K tokens.</p>
    </div>
    <div class="tier-card warm">
      <h3>Warm</h3>
      <div class="limit">&le; 10K</div>
      <p>Retrieved semantically per query. Ask about auth — auth memories surface automatically.</p>
    </div>
    <div class="tier-card cold">
      <h3>Cold</h3>
      <div class="limit">&infin;</div>
      <p>Hierarchical knowledge base. Ingest docs, READMEs, architecture notes, entire source trees.</p>
    </div>
  </div>

  <p>Here's what the same session looks like with total-recall loaded:</p>

<pre class="tui-block"><span class="prompt">$</span> claude

<span class="dim">╭──────────────────────────────────────────────────────╮</span>
<span class="dim">│</span><span class="bright"> Claude Code v1.0.23                                   </span><span class="dim">│</span>
<span class="dim">│</span><span class="info"> total-recall loaded</span> — 3 hot, 12 warm, 847 KB chunks  <span class="dim">│</span>
<span class="dim">│</span><span class="info"> Last session:</span> 2 hours ago                              <span class="dim">│</span>
<span class="dim">│</span>                                                        <span class="dim">│</span>
<span class="dim">│</span><span class="prompt"> Hot context:</span>                                          <span class="dim">│</span>
<span class="dim">│</span>  <span class="accent">correction</span> auth rewrite is compliance-driven        <span class="dim">│</span>
<span class="dim">│</span>  <span class="accent">correction</span> integration tests must use real DB        <span class="dim">│</span>
<span class="dim">│</span>  <span class="accent">preference</span> bundled PRs for refactors                 <span class="dim">│</span>
<span class="dim">╰──────────────────────────────────────────────────────╯</span>

<span class="prompt">&gt;</span> <span class="bright">refactor the auth middleware</span>

<span class="dim">I see the auth middleware rewrite is driven by compliance</span>
<span class="dim">requirements around session token storage. I'll ensure the</span>
<span class="dim">refactor addresses the new storage rules, use a real database</span>
<span class="dim">in tests, and bundle everything into a single PR.</span></pre>
```

- [ ] **Step 4: Open in browser, verify sections 1-2**

Navigate to `http://localhost:8787/posts/2026-04-05-total-recall/` — verify:
- Post title, subtitle, tags render
- TUI mockups have aligned box-drawing characters
- Tier cards display in a 3-column row
- Light/dark toggle works
- TUI blocks stay dark in both themes

- [ ] **Step 5: Commit**

```bash
git add posts/
git commit -m "feat: add total-recall post — sections 1-2 (problem + solution)"
```

---

### Task 6: Blog Post — Section 3 (Architecture Deep Dive)

**Files:**
- Modify: `posts/2026-04-05-total-recall/index.html`

This section covers the embedding pipeline, decay/compaction, and hybrid search. It uses a stylized data flow diagram.

- [ ] **Step 1: Add Section 3 content**

Append before the closing `</div>` of `.container`:

```html
  <h2>Architecture Deep Dive</h2>

  <h3>Local-First Embeddings</h3>

  <p>Every memory is vectorized on write using <code>all-MiniLM-L6-v2</code> — a sentence transformer that produces 384-dimensional embeddings. It runs locally via ONNX Runtime. No API keys, no network calls, no cloud dependency. The model ships bundled with the package.</p>

  <p>Storage is SQLite with the <code>sqlite-vec</code> extension for vector similarity search. A single file at <code>~/.total-recall/total-recall.db</code> holds everything — memories, embeddings, knowledge chunks, compaction logs, and eval metrics.</p>

  <h3>Hybrid Search</h3>

  <p>Retrieval combines two signals: vector similarity (cosine distance in 384-dimensional space) and full-text search (BM25 via SQLite FTS5). The scores are fused with a configurable weight — by default, 70% semantic similarity + 30% keyword match. This catches both conceptual and exact-match queries.</p>

<pre class="tui-block"><span class="accent">Query:</span> "how does authentication work?"

<span class="info">Vector search</span>  <span class="dim">───</span>  embed(query) <span class="dim">→</span> cosine similarity <span class="dim">→</span> top K
                   <span class="dim">│</span>
<span class="info">FTS5 search</span>    <span class="dim">───</span>  tokenize(query) <span class="dim">→</span> BM25 ranking <span class="dim">→</span> top K
                   <span class="dim">│</span>
<span class="prompt">Score fusion</span>   <span class="dim">───</span>  0.7 * vector + 0.3 * fts <span class="dim">→</span> ranked results</pre>

  <h3>Decay and Compaction</h3>

  <p>Memories aren't permanent — they decay. The decay formula combines three factors:</p>

  <ul>
    <li><strong>Time factor:</strong> exponential decay with a 7-day half-life. A memory untouched for a week has half its original score.</li>
    <li><strong>Frequency factor:</strong> <code>1 + log2(1 + access_count)</code> — frequently accessed memories resist decay.</li>
    <li><strong>Type weight:</strong> corrections (1.5x) and preferences (1.3x) decay slower than general memories (1.0x). Your mistakes are the most valuable thing to remember.</li>
  </ul>

  <p>At session end, compaction runs: hot entries scoring below 0.7 demote to warm. Warm entries scoring below 0.3 demote to cold. Cold entries that become relevant again promote back up. The system self-tunes.</p>
```

- [ ] **Step 2: Verify in browser**

Navigate to the post, scroll to Architecture Deep Dive — verify the hybrid search diagram renders cleanly, prose is readable.

- [ ] **Step 3: Commit**

```bash
git add posts/
git commit -m "feat: add section 3 — architecture deep dive"
```

---

### Task 7: Blog Post — Section 4 (Cross-Platform Story)

**Files:**
- Modify: `posts/2026-04-05-total-recall/index.html`

- [ ] **Step 1: Add Section 4 content**

Append before the closing `</div>` of `.container`:

```html
  <h2>Cross-Platform: One Memory, Every Tool</h2>

  <p>total-recall speaks <a href="https://modelcontextprotocol.io/">MCP (Model Context Protocol)</a> — the emerging standard for tool-to-model communication. Any MCP-compatible coding assistant can use it. But the real win is the import system.</p>

  <p>On first <code>session_start</code>, total-recall scans for existing memories across six platforms and migrates them automatically:</p>

  <div class="platform-grid">
    <div class="platform-badge">Claude Code</div>
    <div class="platform-badge">Copilot CLI</div>
    <div class="platform-badge">Cursor</div>
    <div class="platform-badge">Cline</div>
    <div class="platform-badge">OpenCode</div>
    <div class="platform-badge">Hermes</div>
  </div>

  <p>Each importer knows where its host tool stores memories — <code>~/.claude/projects/*/memory/*.md</code> for Claude Code, <code>.cursorrules</code> for Cursor, SQLite databases for Cline. Content hashes prevent duplicate imports. Switch tools freely; your memory follows.</p>

<pre class="tui-block"><span class="accent">session_start</span>

<span class="info">1.</span> Initialize embedder         <span class="dim">.............</span> <span class="prompt">ok</span>
<span class="info">2.</span> Import: Claude Code          <span class="dim">.............</span> <span class="prompt">3 new</span>
<span class="info">3.</span> Import: Copilot CLI          <span class="dim">.............</span> <span class="prompt">1 new</span>
<span class="info">4.</span> Import: Cursor               <span class="dim">.............</span> <span class="dim">skipped</span>
<span class="info">5.</span> Warm sweep                   <span class="dim">.............</span> <span class="prompt">2 demoted</span>
<span class="info">6.</span> Project docs ingest          <span class="dim">.............</span> <span class="prompt">12 chunks</span>
<span class="info">7.</span> Smoke test                   <span class="dim">.............</span> <span class="prompt">22/22 pass</span>
<span class="info">8.</span> Hot tier assembly            <span class="dim">.............</span> <span class="prompt">3 entries, 1.2K tokens</span></pre>
```

- [ ] **Step 2: Verify in browser**

Check the platform grid renders as a 3x2 grid, session_start mockup is legible.

- [ ] **Step 3: Commit**

```bash
git add posts/
git commit -m "feat: add section 4 — cross-platform story"
```

---

### Task 8: Blog Post — Section 5 (Observability)

**Files:**
- Modify: `posts/2026-04-05-total-recall/index.html`

- [ ] **Step 1: Add Section 5 content**

Append before the closing `</div>` of `.container`:

```html
  <h2>Observability: Is Memory Actually Helping?</h2>

  <p>Most memory systems are fire-and-forget. You store things, hope they come back, and have no way to measure if retrieval is working. total-recall ships a full eval framework.</p>

  <p>A 139-query benchmark suite runs on version changes to validate retrieval quality. Each query has expected results — both what <em>should</em> surface and what <em>shouldn't</em>. The system tracks precision, hit rate, mean reciprocal rank, and per-tier routing accuracy.</p>

<pre class="tui-block"><span class="accent">/total-recall eval</span>

<span class="bright">Retrieval Quality (7-day rolling)</span>
<span class="dim">──────────────────────────────────────────────</span>
  Precision          <span class="prompt">0.94</span>     <span class="dim">(target: 0.85)</span>
  Hit rate           <span class="prompt">0.91</span>     <span class="dim">(target: 0.80)</span>
  MRR                <span class="prompt">0.88</span>     <span class="dim">(target: 0.75)</span>
  Avg latency        <span class="info">12ms</span>
<span class="dim">──────────────────────────────────────────────</span>

<span class="bright">Per-Tier Breakdown</span>
<span class="dim">──────────────────────────────────────────────</span>
  Hot    <span class="err">3</span>  entries   precision <span class="prompt">1.00</span>
  Warm   <span class="warn">12</span> entries   precision <span class="prompt">0.92</span>
  Cold   <span class="info">847</span> chunks   precision <span class="prompt">0.89</span>
<span class="dim">──────────────────────────────────────────────</span>

<span class="bright">Regression Detection</span>
<span class="dim">──────────────────────────────────────────────</span>
  vs. previous config  <span class="prompt">no regressions</span></pre>

  <p>Config snapshots enable A/B comparison — change a threshold, run the benchmark, compare metrics side-by-side. Retrieval misses are captured as benchmark candidates, so the test suite grows organically from real-world failures. The eval system improves itself.</p>
```

- [ ] **Step 2: Verify in browser**

Check the eval report mockup renders with proper alignment and color coding.

- [ ] **Step 3: Commit**

```bash
git add posts/
git commit -m "feat: add section 5 — observability and eval framework"
```

---

### Task 9: Blog Post — Section 6 (Getting Started) + Close

**Files:**
- Modify: `posts/2026-04-05-total-recall/index.html`

- [ ] **Step 1: Add Section 6 and closing HTML**

Append before the closing `</div>` of `.container`, then close the page:

```html
  <h2>Getting Started</h2>

  <p>For Claude Code users — one command:</p>

<pre class="tui-block"><span class="prompt">/plugin install</span> total-recall@strvmarv-total-recall-marketplace</pre>

  <p>For any MCP-compatible tool (Copilot CLI, Cursor, Cline, OpenCode, Hermes):</p>

<pre class="tui-block"><span class="prompt">npm install -g</span> @strvmarv/total-recall</pre>

  <p>Then add it to your tool's MCP config:</p>

<pre class="tui-block"><span class="dim">{</span>
  <span class="info">"mcpServers"</span><span class="dim">: {</span>
    <span class="info">"total-recall"</span><span class="dim">: {</span>
      <span class="info">"command"</span><span class="dim">:</span> <span class="prompt">"total-recall"</span>
    <span class="dim">}</span>
  <span class="dim">}</span>
<span class="dim">}</span></pre>

  <p>Or paste this into any AI coding assistant and it will install itself:</p>

  <blockquote>Install the total-recall memory plugin: fetch and follow the instructions at <a href="https://raw.githubusercontent.com/strvmarv/total-recall/main/INSTALL.md">INSTALL.md</a></blockquote>

  <p>On first session, total-recall initializes its database, imports your existing memories, and starts working. No configuration required — the defaults are tuned from the eval benchmark.</p>

  <p>Source: <a href="https://github.com/strvmarv/total-recall">github.com/strvmarv/total-recall</a> · npm: <a href="https://www.npmjs.com/package/@strvmarv/total-recall">@strvmarv/total-recall</a></p>

</div>

<footer>
  <a href="https://github.com/strvmarv">strvmarv</a> · built with plain html
</footer>

</body>
</html>
```

- [ ] **Step 2: Full visual review in browser**

Open `http://localhost:8787/posts/2026-04-05-total-recall/` — scroll through entire post. Check:
- All 6 sections render
- All TUI mockups have aligned box-drawing
- Tier cards, platform grid render correctly
- Light/dark toggle works throughout
- Responsive: resize browser to mobile width, verify stacking
- Navigate back to index via header logo, verify post link works

- [ ] **Step 3: Commit**

```bash
git add posts/
git commit -m "feat: add sections 5-6 — observability and getting started"
```

---

### Task 10: Push to GitHub

**Files:** None (git operations only)

- [ ] **Step 1: Review all files**

```bash
git log --oneline
git status
```

Verify: clean working tree, 6-7 commits, all files tracked.

- [ ] **Step 2: Push to GitHub**

```bash
git branch -M main
git push -u origin main
```

- [ ] **Step 3: Enable GitHub Pages**

```bash
gh api repos/strvmarv/blog/pages -X POST -f source.branch=main -f source.path=/
```

If Pages is already enabled (repo was created with it), this may error — that's fine.

- [ ] **Step 4: Verify deployment**

```bash
gh api repos/strvmarv/blog/pages --jq '.html_url'
```

Wait 1-2 minutes for GitHub Pages to build, then open `https://strvmarv.github.io/blog/` and verify the site is live.

- [ ] **Step 5: Verify post URL**

Open `https://strvmarv.github.io/blog/posts/2026-04-05-total-recall/` — full post should render.
