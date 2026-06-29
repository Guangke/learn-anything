---
name: learn-anything
description: AI-powered deep learning for any topic. Finds the 5 best books, synthesizes a custom textbook with knowledge explanations, code examples, diagrams, and exercises. Supports tree-style exploration (drill into anything unclear), exercise grading, and chapter updates. Use when user wants to master any subject, mentions "learn anything", "学习任何知识", or wants to go from zero to expert on a topic.
---

# Learn Anything

从门外汉到资深水平。不走线性路径——整个过程是树状展开：遇到不懂的直接问，含糊的直接展开讨论直到收敛，然后继续。

## 工作流

### Phase 1：选书

用户提供 TOPIC，列出该领域工业界公认最好的 **5 本书**：

| 书名 | 作者 | 出版年 | 简介 | 适读人群 | 评分 |
|------|------|--------|------|----------|------|
| ... | ... | ... | ... | ... | 豆瓣/GR |

请用户确认或替换书单后再继续。

### Phase 2：合成新书

先输出完整**目录**（每章标注主要来源书籍），等用户确认后逐章生成。

每章固定结构：

```
## 第 N 章：标题
> 来源：《书A》第X章、《书B》第Y章

### 1. 通识与知识讲解
- 知识点：说明 ——《书名》第X章
- 建立概念间关联，说明依赖关系

### 2. 重点辅导

代码示例（注明来源或标注「AI 生成」，写注释）：
\`\`\`language
// 来源：项目XXX / AI 生成
\`\`\`

图表（ASCII 或 Mermaid，注明来源或标注「AI 生成」）

### 3. 习题（5~10 道）
1. [填空] ...
2. [简答] ...
3. [编程] ...
4. [设计] ...
```

代码示例优先使用用户提供的真实项目代码（需用户指定），否则 AI 自行生成并标注。

### Phase 3：章节学习循环

```
[ ] 通读本章
[ ] 遇到不懂 → 说「展开 XXX」→ AI 深入讲解，不限长度
[ ] 讨论收敛 → AI 更新章节，附引用
[ ] 完成习题
[ ] 提交答案 + 自己的理解 → AI 批改
[ ] 指出 AI 讲解的缺失/错误 → AI 更正并完善
[ ] 批改内容追加进章节末尾（标注日期）
[ ] 本章拿捏 → 下一章
```

### Phase 4：巩固记忆

全部章节完成后，每天从头快速过一遍，直到形成长久记忆。

## AI 行为规则

| 触发 | 行为 |
|------|------|
| 用户说「展开 XXX」 | 立即深入讲解，结束时问「清楚了吗？要更新到章节吗？」 |
| 用户提交习题答案 | 评分 + 逐题讲解 + 补充用户理解 + 纠正错误 |
| 发现书中来源 | 格式：`——《书名》第X章` |
| AI 自行生成内容 | 标注：`（AI 生成）` |
| 讨论/批改完成 | 增量内容追加到对应章节末尾，标注日期 |

## 扩展用途

- **备考**：TOPIC 换成考试科目，习题改为历年真题风格
- **面试准备**：TOPIC 换成技术方向，习题改为高频面试题
- **论文调研**：Phase 1 改为找该领域 5 篇最重要的论文或 RFC
