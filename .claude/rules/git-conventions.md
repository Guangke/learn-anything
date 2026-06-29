# Git 提交规范 / Git Commit Conventions

## 分支规范 / Branch Policy

- **所有提交只推送到 `develop` 分支**，不直接推 `master`
- 合并到 `master` 通过 Merge Request 完成

## 标准工作流程 / Standard Workflow

**每次 git 操作都严格按此顺序**：

1. **开始工作前先同步**：`git pull`
2. **如果 pull 失败（本地有未提交修改）**：
   ```bash
   git stash
   git pull
   git stash pop
   # 如有冲突，解决后继续
   ```
3. 修改代码
4. 提交：`git add ... && git commit -m "..."`
5. 推送：`git push`
6. **如果 push 被拒（远端有新提交）**：
   ```bash
   git pull         # 用 merge，不用 --rebase
   # 解决冲突（如有）
   git push
   ```

**禁止项**：
- ❌ 禁止使用 `git pull --rebase`（改写历史）
- ❌ 禁止使用 `git push --force`（覆盖远端）
- ❌ 禁止 commit 后不先 pull 就直接 push

## 提交信息格式 / Commit Message Format

```
<type>: <中文简述>

<可选正文，说明 why 或列举具体变更>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>  # 如有 AI 协助
```

- **标题行**：`type: 中文简述`，不超过 72 字符
- **正文**（可选）：说明"为什么"，列举具体变更点；用 `-` 分项
- **空行**隔开标题和正文

## 类型前缀 / Type Prefixes

| 类型 | 适用场景 |
|------|----------|
| `feat` | 新增功能、新脚本、新接口 |
| `fix` | Bug 修复 |
| `refactor` | 重构、目录调整、不改行为的代码改动 |
| `docs` | 文档、注释、CLAUDE.md 等 |
| `chore` | 依赖更新、配置调整、CI 脚本 |
| `perf` | 性能优化 |
| `test` | 测试相关 |

## 提交语言 / Language

- 标题和正文：**中文**（简述清晰即可）
- 技术词汇、表名、字段名、方法名保留英文原文

## 示例 / Examples

```
feat: 新增文章生产独立脚本，支持取模分片并发
```

```
fix: 修复 tag_materials.py 结果字段名与 TagBatchResult 不匹配导致 exit 1
```

```
refactor: 将 default.yaml 配置合并到各国配置文件，移除全局默认层

- 删除 config/default/default.yaml，各国 test.yaml/prod.yaml 完全自包含
- 更新 src/config.py，移除 default 配置合并逻辑，简化加载流程
- 更新 CLAUDE.md 配置系统说明
```

```
docs: 同步 material_articles 真实表结构，修正 rewrite_articles.interest_id 类型
```

## 不规范示例 / Anti-patterns

```
# ❌ 无前缀
传图域名替换

# ❌ 过于模糊
报错处理

# ❌ 英文 + 无前缀
fix bug

# ❌ 标题说 what，正文没有 why
refactor: 重构配置加载
- 修改了 config.py
- 修改了 default.yaml
```
