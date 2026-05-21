# Marginalia

> Read papers on the left, run Claude Code on the right.
> Turn reading and reasoning into one fluid loop.

[**⬇ Download v0.3.0**](https://github.com/chenhaoqcdyq/marginalia-releases/releases/latest) ·  macOS · Apple Silicon + Intel · [中文 README](README.md)

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
shasum -a 256 Marginalia_0.3.0_universal.dmg
# expected:
# 609098343cae29470a8c665b498fa522c799c2219e0fde03dc585f437a04b6a4
```

---

## License

MIT.

Source code is in a separate private repo. This repo only hosts release binaries and install instructions.

[← 中文 README](README.md)
