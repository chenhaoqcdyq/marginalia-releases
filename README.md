# Marginalia

> 把论文放左边，把 Claude Code 放右边。
> 让阅读和推理变成一个流畅的回路。

[**⬇ 下载 v0.2.0**](https://github.com/chenhaoqcdyq/marginalia-releases/releases/latest) ·  macOS · Apple Silicon + Intel · [English README](README_EN.md)

---

## 简介

**Marginalia** 是一款 macOS 桌面应用——左边读 PDF 论文，右边嵌入了真·Claude Code 终端。
你在 PDF 上划选的文字、论文全文、每篇论文的 Claude 对话历史，会自动注入到 Claude 的上下文里。
不需要复制粘贴、不需要切换窗口、不需要重新讲一遍上下文。

名字来自学术传统中**写在书页边缘的批注**（marginalia）——我们把这件事自动化、规模化，然后接进 AI 对话。

---

## 截图

### 文献库 — 集中管理所有论文
![文献库](screenshots/01-library.png)

- 按文件夹组织（V2A、video、…），可嵌套展开
- 一键导入：本地 PDF / arXiv ID / PDF URL / BibTeX 批量粘贴
- MinerU 解析状态实时显示
- 右键论文：归档 / 重命名 / 删除

### 发现 — HuggingFace 每日 Trending
![Trending](screenshots/02-trending.png)

- 每日刷新 HuggingFace 推荐的 50 篇热门论文
- 自动展示 AI 关键词、上投票数、关联 GitHub
- 一键 `+ Add to library`，PDF 自动从 arXiv 下载

### 阅读 — PDF + Claude Code 并排
![阅读视图](screenshots/03-reader.png)

- 左边连续滚动 PDF，支持双指捏合缩放、文字选区高亮持久化
- 右边嵌入真正的 Claude Code（不是 API 套壳）
- Claude 自动看到你在读哪篇、选了哪段
- **Preview 面板**：渲染 markdown + KaTeX 公式（终端里看不到的，这里能看）
- `+ 新会话` / `历史` 按钮管理每篇论文的多个 Claude 对话

---

## 功能清单

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

---

## 系统要求

- **macOS**（Apple Silicon 或 Intel 都可）
- **Claude Code CLI** 已装并在 `PATH`：参见 [docs.claude.com/claude-code](https://docs.claude.com/claude-code)
- （可选）MinerU API key — 注册 [mineru.net](https://mineru.net/) 拿
- （可选）Semantic Scholar API key — 申请 [semanticscholar.org/product/api](https://www.semanticscholar.org/product/api#api-key-form)

---

## 安装方式

### 1. 下载

到 [Releases](https://github.com/chenhaoqcdyq/marginalia-releases/releases/latest) 下载 **`Marginalia_<version>_universal.dmg`**。

### 2. 安装

打开 dmg → 把 **Marginalia.app** 拖到 **应用程序** 文件夹。

### 3. 首次启动（绕过 Gatekeeper）

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

### 4. 配置

启动后右上角 ⚙ 打开设置 →
- （推荐）粘贴 MinerU API key + 勾"导入论文时自动用 MinerU 解析"
- （推荐）粘贴 Semantic Scholar API key（避开匿名限流）
- 关闭设置 → 可以用了

---

## 一些"暗坑"提醒

- **数据存在 `~/.alphaxiv++/`**（历史遗留名，下个大版本迁移到 `~/.marginalia/`）—— 想备份就备份这个目录，或在设置里点"导出文献库"
- **Claude 会话**存在 `~/.claude/projects/<encoded-cwd>/*.jsonl`，由 Claude Code 自己管，跟应用同步走
- **第一次解析**：导论文后 MinerU 后台跑 30 秒到几分钟，不阻塞，右下角 toast 报进度

---

## 校验下载

```bash
shasum -a 256 Marginalia_0.2.0_universal.dmg
# 期望值：
# 774c276b618ab343124193bea0fee8d4a50e60f298d8e700c741df1aca292221
```

---

## License

MIT.

源代码在独立的私有 repo。本 repo 只托管构建产物 + 安装指南。

[English README →](README_EN.md)
