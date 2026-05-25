# Marginalia

> **Read papers with an AI research assistant who actually remembers what you've read.**

Marginalia embeds Claude Code into a macOS PDF reader where it plays two
roles at the same time:

- 📖 **Reading partner** — Claude knows which paper you're on, what you just
  selected, every highlight you've made, and every prior chat about it.
  Ask "explain this notation", "summarize §3", "is this claim novel?" —
  **zero copy-paste, zero context-reset**.
- 📚 **Library librarian** — an `Ask Claude` tab lets Claude see your **whole
  library**. Ask "which paper used contrastive loss?", "compare these three
  diffusion methods", "find me everything about MoE" — Claude's built-in
  `Read` / `Glob` / `Grep` search across your library + MinerU-parsed full
  text. No MCP server, no custom tools.
- 🖍 **Notes stay local** — 7-color highlights with inline notes, per-paper
  Claude session history, one-click zip backup. **Everything lives on your
  disk**, not a cloud silo.

[**⬇ Download v0.6.0**](https://github.com/chenhaoqcdyq/marginalia-releases/releases/latest) ·  macOS · Apple Silicon + Intel · [中文 README](README.md)

---

## 🆕 News — v0.6.0 (2026-05-24)

**Library-level Claude chat** + a paper-scope picker + browser-style chat
tabs + pop-out chat windows. Run several Claude sessions in parallel
against different scopes — one chatting about a specific paper, another
about your whole V2A folder, no crowding.
[Full release notes →](https://github.com/chenhaoqcdyq/marginalia-releases/releases/tag/v0.6.0)

- 💬 **Ask Claude tab** in the main library — a fourth top-level tab that
  drops you straight into a Claude conversation about your **whole library**.
  A synthetic workdir `~/.alphaxiv++/papers/library-index/` is materialized
  with `INDEX.md` + per-paper `meta.json` + symlinks to MinerU markdown, so
  Claude's built-in `Read` / `Glob` / `Grep` "just work" on the library —
  no MCP server, no custom tools.
- 🎯 **Scope picker** — the rose pill in the chat header (`📚 N papers ▾`)
  opens a popover with three presets:
  - **Custom selection** (default): pick folders + individual papers with
    a searchable list. Drag the popover's bottom edge to resize.
  - **Entire library**: preserves v0.5.x behaviour.
  - **Recently opened N**: most recently opened N papers (default 20).
  Only the chosen subset is materialized. Selection persists across
  reloads; `INDEX.md` gets a `_Scope:_` line so Claude knows which subset
  it sees.
- 🗂 **Browser-style chat tabs**: `+` in the chat header opens a fresh tab
  alongside the current one. Each tab has its own session id and
  `localStorage` namespace, survives reloads, keeps running in the
  background when not active (mounted + hidden via CSS so streams
  continue), and is labelled with a truncated preview of its most recent
  question. Close any with `×`; closing the last auto-creates a fresh tab.
- 🪟 **Pop-out chat windows** — new `open_chat_window` Rust command spawns
  an independent macOS window for any chat (label `chat-…`) so you can run
  library + paper chats side-by-side without crowding the reader.
- 🛠 Fixes: library SQL `archived_at` predicate was inverted (selecting
  tentative previews instead of the real library), so library chat now sees
  the actual folder groupings; chat scroll bug (two parent containers were
  missing `min-h-0`) was unbounding the messages area.

---

## 🆕 News — v0.5.0 (2026-05-24)

In-PDF **citation cards** + a redesigned **chat preview**. Click `[N]`
and a paper card pops over the spot — title, reference text, first-page
thumbnail, Jump, Bookmark, Open-in-Marginalia. Click `Figure 3` /
`Table 2` and you skip straight to the figure body with a "← Back to
page N" pill.
[Full release notes →](https://github.com/chenhaoqcdyq/marginalia-releases/releases/tag/v0.5.0)

- 🎯 **Citation cards on click**: PDF stops jumping away. Card follows
  the PDF as you scroll / zoom, dismisses on `Esc` or click-outside.
- 🧠 **Smart Fig / Table / Section / Algorithm / Theorem detection**:
  non-citation links bypass the card and do a direct jump to the
  figure / table BODY (auto-shifted up so the actual content is in
  view, not just the caption), with a floating "← Back to page N" pill.
- 🖼 **First-page thumbnail in the card**: instant when the cited paper
  is in your library; otherwise streaming download from arXiv with a
  real progress bar; cached to disk.
- ➡ **Red → button: open in Marginalia** — finds the cited paper in
  your library or downloads + imports it, then opens it in a new
  reader window. Loading overlay with a real progress bar and a
  cancel button so a 10 MB download doesn't lock you in.
- 🔖 **Bookmark from the card**: pick existing folders or create one
  inline. Atomically imports the paper if needed and writes the
  folder assignments.
- 📚 **Reference parsing**: MinerU's `paper.md` is parsed into a
  numbered reference list. `[N]` resolves against destination text
  (sidesteps the `[9, 10, 11]` cluster ambiguity that source-side
  parsing used to get wrong).
- 💬 **Chat preview redesign**: user turns render as right-aligned
  rose chat bubbles with Q1/Q2 numbering, visually distinct from
  the assistant's markdown replies. Header nav switches to icon-only
  `« ‹ › »` buttons; `‹ Prev Q` is two-stage (snaps to the top of
  the current question first if mid-scroll, then steps back on a
  second tap). System-injected `<system-reminder>` / tool_result /
  hook messages are filtered out so only what you actually typed
  shows.
- 🛠 **PDFView lifecycle fixes**: back-to-library now atomically
  removes the PDFView from the view tree (some compositor states
  used to leak the previous paper's pages onto the library UI);
  re-opening flushes pending frames so PDFView reappears on first try.
- ⚙ **Multi-webview Tauri plumbing**: every `pdfkit_*` command that
  the main reader hits after the citation card overlay is added now
  uses `tauri::Webview` instead of `WebviewWindow` (the latter
  rejects the parent webview with "current webview is not a
  WebviewWindow" once `add_child` makes the window a multi-webview
  composite).

---

## 🆕 News — v0.4.0 (2026-05-22)

Native macOS **PDFKit renderer** by default, with a full annotation
layer on top.
[Full release notes →](https://github.com/chenhaoqcdyq/marginalia-releases/releases/tag/v0.4.0)

- 🖥 **PDFKit renderer default on macOS**: native pinch / scroll /
  selection, lower memory, sharper text at every zoom; PDF.js stays
  as the cross-platform fallback.
- 🖍 **Annotations**: select text → pick from a 7-color palette →
  optional note → Highlight. Stored as
  `~/.alphaxiv++/papers/<id>/annotations.json`, repainted via real
  `PDFAnnotation`s on next open. Click any existing highlight to
  load it back into the ribbon for edit / delete; ⌘↵ saves.
- 🎈 **Hover marginalia bubble**: hover a highlight → small bubble
  pops out at the right edge of the PDF column with page + selected
  text preview + note.
- 💡 **Ask AI button**: writes the highlight + note to
  `current_selection.md` so the next Claude prompt has full context.
- 🧰 Fixes: native PDFView no longer covers the React header
  (`contentLayoutRect` Y flip); NSString conversion panic in objc2 0.6;
  re-open after "back to library" silently focused stale library
  windows.

---

## 🆕 News — v0.3.0 (2026-05-21)

Major release: HF Daily panel redesign, broader search backends, smart
caching, instant paper opens.
[Full release notes →](https://github.com/chenhaoqcdyq/marginalia-releases/releases/tag/v0.3.0)

- 🤗 **HF Daily panel redesign**: Daily / Weekly / Monthly range tabs,
  Top / Hot / Newest sort, date browser, submitter avatars, discussion
  deep-links, thumbnail lightbox, expandable abstracts, click-to-filter
  keyword chips, optional HuggingFace token.
- ⚡ **Per-day cache shared across ranges + background prefetch**: after
  one Monthly fetch, switching to Weekly / Daily is free; today's data
  is silently refreshed every 10 min.
- 🔎 **OpenAlex** as a third search backend (no API key, 100k req/day/IP).
  Now the default.
- ⚡ **Optimistic open-reader**: clicking a Trending paper opens the
  reader immediately with a "downloading…" placeholder; PDF + DB insert
  run in the background.
- 🔄 **State persistence**: Library / Search / Trending tabs each remember
  scroll, query, filter, range. Sidebar nav rows auto-switch to My Library.
- ⚙ **Settings panel redesign**: bubble cards, network diagnostics as a
  2-col card grid with status dots / latency / expandable errors, manual
  proxy override live-applied.
- 🖼 **PDF sharpness setting** (default 2× = Retina native, matches macOS
  Preview); Modern/Legacy worker toggle for older Intel Mac Safari.
- 🆕 **New app icon**: cream squircle + crimson margin mark.
- 🔔 **In-app update check**: silent daily check against GitHub Releases
  with ETag caching and 6h backoff on rate-limit.

---

## Overview

**Marginalia** is a macOS desktop application: PDF on the left, **embedded Claude Code** on the right. Your text selections, the paper's full content, and per-paper conversation history are automatically injected into Claude's context. No copying, no window switching, no re-explaining your context.

The name nods to the scholarly tradition of writing notes **in the margins of a book** — we automate, scale, and route those notes into a live AI conversation.

---

## Screenshots

### Library — organize all your papers
![Library](screenshots/01-library.png)

- Folder-organized (V2A, video, …) with expandable tree in the sidebar
- One-click import: local PDF / arXiv ID / PDF URL / BibTeX paste
- Live MinerU parse status per row
- Right-click for archive / rename / delete

### Discovery — HuggingFace Daily Papers
![Trending](screenshots/02-trending.png)

- **Daily / Weekly / Monthly** range tabs, **Top / Hot / Newest** sort
- ◀ ▶ date browser for any past day's papers
- Submitter avatars (click → HF profile), 💬 discussion deep-links,
  click-to-zoom thumbnail lightbox, click-to-filter keyword chips
- **Per-day cache shared across ranges**: after a Monthly fetch, switching
  to Weekly / Daily is free. Background prefetch refreshes today every
  10 min while the panel is mounted.
- One-click `+ Add to library` or **click the title to open the reader**
  instantly (PDF downloads in the background, no blank wait)
- Optional **HuggingFace token** in Settings to enter HF's polite pool
  and dodge the Monthly rate-limit risk

### Reader — PDF + Claude Code side by side
![Reader](screenshots/03-reader.png)

- Continuous-scroll PDF with pinch-to-zoom and persistent text-selection highlight
- The **real** Claude Code on the right (not an API wrapper)
- Claude auto-sees which paper you're reading and what you've selected
- **Preview panel**: renders markdown + KaTeX formulas (which the terminal can't)
- `+ New` / `History` buttons manage multiple Claude conversations per paper

---

## Features

- ✅ **Three-pane reader**: folder sidebar · PDF viewer · Claude Code terminal
- ✅ **Library**: SQLite-backed with folders, archived/tentative state, recent view
- ✅ **Paper sources**:
  - Local PDF upload
  - arXiv ID / URL
  - Any PDF URL
  - **BibTeX paste** (bulk, background-imports, deduped by arxiv_id, files into any folder)
  - **OpenAlex (default, no key, 100k req/day/IP)** / Semantic Scholar /
    arXiv search; per-query result cache so switching tabs is instant
  - 🤗 HuggingFace Daily Papers (Daily / Weekly / Monthly + multiple sorts)
- ✅ **PDF viewer**: continuous scroll · pinch-to-zoom (WebKit gestures) · page virtualization (100-page papers don't blow up memory) · **persistent text-selection highlight** (CSS Custom Highlight API — stays visible after you click Claude)
- ✅ **Per-paper Claude sessions**: `claude -c` to continue · `+ New` for a fresh session · `History ▾` to resume any past conversation with full transcript
- ✅ **Markdown + KaTeX preview pane**: renders formulas in-place, instead of you reading `$$\sum$$` literally
- ✅ **MinerU integration** (optional, opt-in): cloud-parses PDFs into structured markdown with LaTeX formulas + extracted figures, runs in background
- ✅ **Selection → Claude auto-injection**: highlight on PDF → file write → `UserPromptSubmit` hook injects it into the next prompt, no copy-paste
- ✅ **Library export / import**: one-button .zip backup including PDFs, MinerU output, and Claude jsonl session files
- ✅ **Settings dialog**: API keys (MinerU / Semantic Scholar / HuggingFace),
  Claude pane theme (6 presets), PDF render sharpness, PDF render mode
  (modern / legacy compat), manual proxy override, network diagnostics
  (card grid with status dots), backup/restore
- ✅ **In-app update check**: silent daily check against GitHub Releases,
  header badge when newer; manual "Check now" in Settings; ETag-cached
  to avoid GitHub rate limits

---

## Requirements

- **macOS** (Apple Silicon or Intel)
- **Claude Code CLI** installed and on `PATH` — see [docs.claude.com/claude-code](https://docs.claude.com/claude-code)
- (optional) MinerU API key — sign up at [mineru.net](https://mineru.net/)
- (optional) Semantic Scholar API key — apply at [semanticscholar.org/product/api](https://www.semanticscholar.org/product/api#api-key-form)

---

## Install

### 1. Download

Get **`Marginalia_<version>_universal.dmg`** from [Releases](https://github.com/chenhaoqcdyq/marginalia-releases/releases/latest).

### 2. Install

Open the dmg → drag **Marginalia.app** into the **Applications** folder.

### 3. First launch (Gatekeeper bypass)

The build is **not Apple-code-signed** (no Apple Developer account yet), so on first launch macOS will say:

> *"Marginalia" can't be opened because Apple cannot check it for malicious software.*

**Option A — Right-click open (one-time)**
1. Open the **Applications** folder
2. **Right-click** (or Ctrl-click) `Marginalia` → choose **Open**
3. The dialog changes to one with an **Open** button → click **Open**
4. Double-clicking works normally from now on.

**Option B — Terminal**
```bash
xattr -dr com.apple.quarantine /Applications/Marginalia.app
```
Then double-click as normal.

### 4. Configure

Launch the app → click ⚙ at top-right →
- (recommended) Paste your MinerU API key + tick "Enable MinerU auto-parse on import"
- (recommended) Paste your Semantic Scholar API key (avoids anonymous rate limits)
- Close settings — you're set.

---

## Notes & gotchas

- **Library data** lives at `~/.alphaxiv++/` (legacy path; will migrate to `~/.marginalia/` in v1.0). To back up, copy that directory or use Settings → Backup → Export Library.
- **Claude conversation history** lives at `~/.claude/projects/<encoded-cwd>/*.jsonl`, managed by Claude Code itself — moves with the app.
- **First MinerU parse** runs 30 s – a few minutes per paper in background, non-blocking; progress toasts appear in the bottom-right.

---

## Verify download

```bash
shasum -a 256 Marginalia_0.6.0_universal.dmg
# expected:
# 00bbbd02570971f1886bb140a5cddcfbaf0e52655f8fcaae8bc9a1619b204c9f
```

---

## License

MIT.

Source code is in a separate private repo. This repo only hosts release binaries and install instructions.

[← 中文 README](README.md)
