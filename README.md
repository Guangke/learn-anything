# Learn Anything — AI辅助快速学习法

**从门外汉到资深水平。不需要看书，让 AI 替你读完，然后你来学。**

Works with Claude Code, Codex, Cursor, Gemini CLI, and more.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-8A2BE2)](https://docs.anthropic.com/en/docs/claude-code)

[English](#english) | [简体中文](#简体中文)

---

## 简体中文

### 核心理念

和传统线性学习路径完全不同——整个过程是**树状展开**：

- 不用管知识的循序渐进，直接看到不懂的就问
- 遇到含糊不清或细节不够就直接展开讨论，直到收敛
- 做完习题让 AI 批改，如果 AI 有错误就指出来，让它更正

目标：从门外汉到资深水平。

### 工作流概览

```
Phase 1  找出该 TOPIC 工业界最好的 5 本书
   ↓
Phase 2  根据 5 本书合成一本新书（目录 → 逐章生成）
   ↓        每章 = 通识讲解 + 重点辅导（代码+图表）+ 习题
Phase 3  逐章学习循环
   ↓        展开讨论 → 收敛 → 更新章节 → 做题 → 批改 → 拿捏
Phase 4  全章节完成 → 每天快速复习，直到形成长久记忆
```

### 快速开始

**Claude Code：**

```bash
/plugin install Guangke/learn-anything
```

然后：

```
/learn-anything Redis
```

**其他工具（Codex / Cursor / Gemini CLI 等）：**

将 `.claude/skills/learn-anything/SKILL.md` 内容直接粘贴到对话中，或 clone 本仓库后按各平台方式加载 skill。

**手动 Prompt（任意 AI 工具）：**

```
请按照 learn-anything 工作流帮我学习 <TOPIC>。
第一步找出该领域最好的5本书；第二步根据这5本书合成一本新书（含目录和完整章节）；
每章分为：通识讲解（注明书籍来源）、重点辅导（含代码和图表）、习题；
然后逐章学习，遇到不懂的我会说"展开 XXX"，你深入讲解直到收敛并更新章节；
习题我提交答案后你批改并更新章节。
```

### 平台兼容

| 平台 | 状态 | 安装方式 |
|------|------|----------|
| Claude Code | ✅ | `/plugin install Guangke/learn-anything` |
| Cursor | ✅ | Clone 后将 skill 文件放入 `.cursor/rules/` |
| Gemini CLI | ✅ | Clone 后将 skill 文件放入 `~/.gemini/skills/` |
| Codex | ✅ | Clone 后将 skill 文件放入 `~/.codex/skills/` |
| 任意 AI 工具 | ✅ | 直接粘贴 SKILL.md 内容到对话 |

### 扩展用途

- **快速备考**：TOPIC 换成考试科目，习题改为历年真题风格
- **面试准备**：TOPIC 换成技术方向，习题改为高频面试题
- **论文调研**：Phase 1 改为找该领域 5 篇最重要的论文或 RFC

---

## English

### What is this?

Learn Anything is an AI-powered learning skill that takes any topic from zero to expert level — without reading books yourself.

Instead of linear learning, it works like a **decision tree**: dive into anything you don't understand, expand the discussion until it converges, then keep going.

### How it works

```
Phase 1  Find the 5 best books in the field for your TOPIC
   ↓
Phase 2  Synthesize a new book from those 5 (TOC → chapter by chapter)
   ↓        Each chapter = knowledge overview + guided deep-dive (code + diagrams) + exercises
Phase 3  Chapter-by-chapter study loop
   ↓        Expand unclear topics → converge → update chapter → exercises → grading → mastered
Phase 4  All chapters done → daily review until long-term memory forms
```

### Quick Start

**Claude Code:**

```bash
/plugin install Guangke/learn-anything
```

Then:

```
/learn-anything Redis
```

**Other tools:** Paste the contents of `.claude/skills/learn-anything/SKILL.md` directly into your conversation.

---

## License

MIT © Guangke
