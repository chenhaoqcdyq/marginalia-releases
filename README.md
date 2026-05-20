# Marginalia

> 把论文放左边，把 Claude Code 放右边。让阅读和推理变成一个流畅的回路。
> Read papers on the left, run Claude Code on the right.
> Turn reading and reasoning into one fluid loop.

[**⬇ Download v0.2.0**](https://github.com/chenhaoqcdyq/marginalia-releases/releases/latest) ·  macOS · Apple Silicon + Intel

---

## 🇨🇳 中文

### 简介

**Marginalia** 是一款 macOS 桌面应用——左边读 PDF 论文，右边嵌入了真·Claude Code 终端。
你在 PDF 上划选的文字、论文全文、每篇论文的 Claude 对话历史，会自动注入到 Claude 的上下文里。
不需要复制粘贴、不需要切换窗口、不需要重新讲一遍上下文。

名字来自学术传统中**写在书页边缘的批注**（marginalia）——我们把这件事自动化、规模化，然后接进 AI 对话。

### 截图

#### 文献库 — 集中管理所有论文
![Library](screenshots/01-library.png)

- 按文件夹组织（V2A、video、…）
- 一键导入：本地 PDF / arXiv ID / PDF URL / BibTeX 批量粘贴
- MinerU 解析状态实时显示
- 右键论文：归档 / 重命名 / 删除

#### 发现 — HuggingFace 每日 Trending
![Trending](screenshots/02-trending.png)

- 每日刷新 HuggingFace 推荐的 50 篇热门论文
- 自动展示 AI 关键词、上投票数、关联 GitHub
- 一键 `+ Add to library`，PDF 自动从 arXiv 下载

#### 阅读 — PDF + Claude Code 并排
![Reader](screenshots/03-reader.png)

- 左边连续滚动 PDF，支持双指捏合缩放、文字选区高亮持久化
- 右边嵌入真正的 Claude Code（不是 API 套壳）
- Claude 自动看到你在读哪篇、选了哪段
- **Preview 面板**：渲染 markdown + KaTeX 公式（终端里看不到的，这里能看）
- `+ 新会话` / `历史` 按钮管理每篇论文的多个 Claude 对话

### 功能清单

- ✅ **三栏阅读器**：文件夹侧栏 · PDF 查看器 · Claude Code 终端
- ✅ **文献库**：SQLite 后端，支持文件夹、归档、最近阅读、临时预览（点搜索结果不入库）
- ✅ **多来源导入**：
  - 本地 PDF 拖入
  - arXiv ID / URL 一键导入
  - 任意 PDF URL 下载
  - **BibTeX 批量粘贴**（后台导入，自动按 arxiv_id 去重，归类到任意文件夹）
  - Semantic Scholar 搜索（推荐）/ arXiv 搜索（备选）
  - 🤗 HuggingFace 每日 Trending
- ✅ **PDF 查看器**：连续滚动 · 双指捏合缩放（WebKit 手势）· 页面虚拟化（开 100 页论文不爆内存）· 文字选区**持久高亮**（点别处也不消失）
- ✅ **每论文独立 Claude 会话**：`claude -c` 自动续上次对话，`+ 新会话` 开新，`历史 ▾` 恢复任意历史会话并看完整 transcript
- ✅ **Markdown + KaTeX preview pane**：Claude 回复的公式实时渲染，不再是 `$$\sum$$` 字面字符
- ✅ **MinerU 集成**（可选）：云端解析 PDF 为结构化 markdown（LaTeX 公式 + 真表格 + 抽取图表），后台跑不阻塞
- ✅ **选区 → Claude 自动注入**：PDF 上划选 → `current_selection.md` 写文件 → UserPromptSubmit hook 注入到下条 prompt → Claude 自然知道你在问哪段
- ✅ **导出 / 导入文献库**：一键打包成 zip（含 PDF、MinerU 结果、Claude jsonl），换机器或备份用
- ✅ **设置面板**：填 MinerU / Semantic Scholar API key、重新提取所有论文标题、备份恢复

### 系统要求

- **macOS**（Apple Silicon 或 Intel 都可）
- **Claude Code CLI** 已装并在 `PATH`：参见 [docs.claude.com/claude-code](https://docs.claude.com/claude-code)
- （可选）MinerU API key — 注册 [mineru.net](https://mineru.net/) 拿
- （可选）Semantic Scholar API key — 申请 [semanticscholar.org/product/api](https://www.semanticscholar.org/product/api#api-key-form)

### 安装方式

#### 1. 下载

到 [Releases](https://github.com/chenhaoqcdyq/marginalia-releases/releases/latest) 下载 **`Marginalia_<version>_universal.dmg`**。

#### 2. 安装

打开 dmg → 把 **Marginalia.app** 拖到 **应用程序** 文件夹。

#### 3. 首次启动（绕过 Gatekeeper）

应用**未做 Apple 代码签名**（还没买 Apple Developer 账号），所以首次双击会弹：
> *"Marginalia" 无法打开，因为 Apple 无法检查它是否包含恶意软件*

**方法 A — 右键打开（一次性放行）**
1. 打开**应用程序**文件夹
2. **右键**（或按住 Control 点击）`Marginalia` → 选 **打开**
3. 弹窗变成有 **打开** 按钮的版本 → 点 **打开**
4. 之后双击就行

**方法 B — 终端命令**
```bash
xattr -dr com.apple.quarantine /Applications/Marginalia.app
```
然后正常双击。

#### 4. 配置

启动后右上角 ⚙ 打开设置 →
- （推荐）粘贴 MinerU API key + 勾"导入论文时自动用 MinerU 解析"
- （推荐）粘贴 Semantic Scholar API key（避开匿名限流）
- 关闭设置 → 可以用了

### 一些"暗坑"提醒

- **数据存在 `~/.alphaxiv++/`**（历史遗留名，下个大版本迁移到 `~/.marginalia/`）—— 想备份就备份这个目录，或在设置里点"导出文献库"
- **Claude 会话**存在 `~/.claude/projects/<encoded-cwd>/*.jsonl`，由 Claude Code 自己管，跟应用同步走
- **第一次解析**：导论文后 MinerU 后台跑 30 秒到几分钟，不阻塞，右下角 toast 报进度

---

## 🇬🇧 English

### Overview

**Marginalia** is a macOS desktop application: PDF on the left, **embedded Claude Code** on the right. Your text selections, the paper's full content, and per-paper conversation history are automatically injected into Claude's context. No copying, no window switching, no re-explaining your context.

The name nods to the scholarly tradition of writing notes **in the margins of a book** — we automate, scale, and route those notes into a live AI conversation.

### Screenshots

#### Library
![Library](screenshots/01-library.png)

Folder-organized library · multi-source import · live MinerU parse status · right-click actions.

#### Discovery — HuggingFace Trending
![Trending](screenshots/02-trending.png)

Daily-refreshed HuggingFace top 50 · AI-extracted keywords · one-click add (PDF auto-downloaded from arXiv).

#### Reader — PDF + Claude Code
![Reader](screenshots/03-reader.png)

Continuous-scroll PDF with pinch-zoom · persistent selection highlight · the **real** Claude Code on the right (not an API wrapper) · markdown + KaTeX preview pane for formula rendering · per-paper session history.

### Features

- ✅ **Three-pane reader**: folder sidebar · PDF viewer · Claude Code terminal
- ✅ **Library**: SQLite-backed with folders, archived/tentative state, recent view
- ✅ **Paper sources**:
  - Local PDF upload
  - arXiv ID / URL
  - Any PDF URL
  - **BibTeX paste** (bulk, background-imports, deduped by arxiv_id, files into any folder)
  - Semantic Scholar / arXiv search
  - 🤗 HuggingFace Daily Trending
- ✅ **PDF viewer**: continuous scroll · pinch-to-zoom (WebKit gestures) · page virtualization (100-page papers don't blow up memory) · **persistent text-selection highlight** (CSS Custom Highlight API — stays visible after you click Claude)
- ✅ **Per-paper Claude sessions**: `claude -c` to continue · `+ New` for a fresh session · `History ▾` to resume any past conversation with full transcript
- ✅ **Markdown + KaTeX preview pane**: renders formulas in-place, instead of you reading `$$\sum$$` literally
- ✅ **MinerU integration** (optional, opt-in): cloud-parses PDFs into structured markdown with LaTeX formulas + extracted figures, runs in background
- ✅ **Selection → Claude auto-injection**: highlight on PDF → file write → `UserPromptSubmit` hook injects it into the next prompt, no copy-paste
- ✅ **Library export / import**: one-button .zip backup including PDFs, MinerU output, and Claude jsonl session files
- ✅ **Settings dialog**: API keys (MinerU + Semantic Scholar), bulk title re-extraction, backup/restore

### Requirements

- **macOS** (Apple Silicon or Intel)
- **Claude Code CLI** installed and on `PATH` — see [docs.claude.com/claude-code](https://docs.claude.com/claude-code)
- (optional) MinerU API key — sign up at [mineru.net](https://mineru.net/)
- (optional) Semantic Scholar API key — apply at [semanticscholar.org/product/api](https://www.semanticscholar.org/product/api#api-key-form)

### Install

#### 1. Download

Get **`Marginalia_<version>_universal.dmg`** from [Releases](https://github.com/chenhaoqcdyq/marginalia-releases/releases/latest).

#### 2. Install

Open the dmg → drag **Marginalia.app** into the **Applications** folder.

#### 3. First launch (Gatekeeper bypass)

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

#### 4. Configure

Launch the app → click ⚙ at top-right →
- (recommended) Paste your MinerU API key + tick "Enable MinerU auto-parse on import"
- (recommended) Paste your Semantic Scholar API key (avoids anonymous rate limits)
- Close settings — you're set.

### Notes

- **Library data** lives at `~/.alphaxiv++/` (legacy path; will migrate to `~/.marginalia/` in v1.0). To back up, copy that directory or use Settings → Backup → Export Library.
- **Claude conversation history** lives at `~/.claude/projects/<encoded-cwd>/*.jsonl`, managed by Claude Code itself — moves with the app.
- **First MinerU parse** runs 30 s – a few minutes per paper in background, non-blocking; progress toasts appear in the bottom-right.

---

## License

MIT.

Source code is in a separate private repo. This repo only hosts release binaries and install instructions.
