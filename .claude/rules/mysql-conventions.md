# MySQL 规范 / MySQL Conventions

## 禁止项 / Prohibitions

- **禁止 FOREIGN KEY 约束**：关联完整性由应用层保证，不在数据库层声明
- **禁止 `CREATE INDEX` 语句**：索引必须写在 `ALTER TABLE` 里
- **每张表的所有 DDL 变更合并为一条 `ALTER TABLE`**，不拆多条
- **禁止使用 `enum`/`set` 类型**：用 `VARCHAR` 或 `TINYINT` 替代
- **禁止使用 `procedure`/`function`/`trigger`/`view`/`event`**

## 字段类型规范 / Column Types

| 场景 | 类型 |
|------|------|
| 主键 | `BIGINT UNSIGNED AUTO_INCREMENT` |
| 关联 ID（逻辑外键） | `BIGINT UNSIGNED NOT NULL` |
| 状态枚举 | `VARCHAR(20~50) NOT NULL DEFAULT 'pending'` |
| 短字符串（名称、类型）| `VARCHAR(50~200)` |
| URL、路径 | `VARCHAR(500)` |
| 长文本 | `TEXT` |
| JSON 结构 | `JSON`（MySQL 5.7.8+）|
| 时间戳（UTC 秒）| `INT UNSIGNED NOT NULL DEFAULT 0` |
| 时间戳（datetime）| `TIMESTAMP DEFAULT CURRENT_TIMESTAMP` |
| 布尔/标志位 | `TINYINT(1) NOT NULL DEFAULT 0` |

## 建表规范 / CREATE TABLE

```sql
CREATE TABLE `rewrite_tasks` (
  `id`           BIGINT UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键 / Primary key',
  `interest_id`  BIGINT UNSIGNED NOT NULL                COMMENT '关联兴趣 ID / Associated interest ID',
  `task_status`  VARCHAR(20)     NOT NULL DEFAULT 'pending' COMMENT '任务状态: pending/running/done/failed',
  `create_time`  INT UNSIGNED    NOT NULL DEFAULT 0       COMMENT '创建时间 UTC 秒 / Created at UTC seconds',
  `update_time`  INT UNSIGNED    NOT NULL DEFAULT 0       COMMENT '更新时间 UTC 秒 / Updated at UTC seconds',
  PRIMARY KEY (`id`),
  KEY `idx_interest_id` (`interest_id`),
  KEY `idx_task_status` (`task_status`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='洗稿父任务表 / Rewrite parent tasks';
```

- 每列必须有 `COMMENT`，中英双语
- 表必须有 `COMMENT`，中英双语
- 字符集统一 `utf8mb4`，引擎统一 `InnoDB`
- 索引命名：`idx_<字段名>` 或 `idx_<字段1>_<字段2>`
- 唯一索引命名：`uniq_<字段名>`

## 变更规范 / ALTER TABLE

**每张表只写一条 `ALTER TABLE`**，把所有列变更和索引合并进去：

```sql
-- ✅ 正确：合并为一条
ALTER TABLE `material_articles`
  ADD COLUMN `published`     TINYINT(1)      NOT NULL DEFAULT 0 COMMENT '是否已发布 / Published flag',
  ADD COLUMN `quality_score` DECIMAL(5,2)    NOT NULL DEFAULT '0.00' COMMENT '质量评分 / Quality score',
  ADD INDEX  `idx_published` (`published`),
  ADD INDEX  `idx_quality`   (`quality_score`);

-- ❌ 错误：拆成多条
ALTER TABLE `material_articles` ADD COLUMN `published` TINYINT(1) NOT NULL DEFAULT 0;
ALTER TABLE `material_articles` ADD INDEX `idx_published` (`published`);
```

## Alembic 迁移规范 / Alembic Migration Conventions

### 文件命名

```
{YYYYMMDD}_{HHMM}_{seq}_{slug}.py
# 示例
20260410_1700_012_add_rewrite_workflow_tables.py
```

### 文件头注释

```python
"""012_add_rewrite_workflow_tables

中文说明：新增洗稿工作流三张表。
English: Add three tables for rewrite workflow.

Revision ID: 012_rewrite_workflow
Revises: 011_xxx
Create Date: 2026-04-10 17:00:00
"""
```

### upgrade / downgrade

- 每个逻辑块加中英双语注释
- `downgrade` 必须实现，顺序与 `upgrade` 相反
- 用 `op.create_table` / `op.add_column` / `op.create_index`，不拼 raw SQL

```python
def upgrade() -> None:
    # 1. 新增 published 字段 / Add published column
    op.add_column('material_articles',
        sa.Column('published', mysql.TINYINT(), nullable=False,
                  server_default='0',
                  comment='是否已发布（0=否，1=是）/ Published flag'))
    op.create_index('idx_published', 'material_articles', ['published'])

def downgrade() -> None:
    op.drop_index('idx_published', table_name='material_articles')
    op.drop_column('material_articles', 'published')
```

## SQLAlchemy 模型规范 / Model Conventions

- 每列加 `comment=` 参数，中文说明
- 主键：`Column(BigInteger, primary_key=True, autoincrement=True)`
- 不声明 `ForeignKey()`（无外键约束规范）
- 关联查询走应用层 JOIN，不用 `relationship()`（历史代码有，新代码不加）
- 表名：全小写下划线，如 `material_articles`、`rewrite_tasks`
