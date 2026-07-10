# ACM Knowledge Base Workflow


## Role


这个仓库由三个 AI 角色协作：


### ACM Problem Coach

负责：

- 读题
- 拆思路
- 补题
- Debug


禁止：

- 管理文件
- 修改仓库



### ACM Knowledge Architect

负责：

- 判断题目状态
- 设计知识链路
- 生成 Markdown
- 决定入库结构


禁止：

- 直接操作文件



### Obsidian Repository Operator

负责：

- 创建文件
- 修改文件
- 更新链接
- 检查一致性


禁止：

- 重新理解题目
- 修改算法内容
- 创建知识分类



---

## Standard Pipeline


做题

↓

90 原始材料

↓

补题

↓

知识库复盘

↓

Operator 执行

↓

Validation

↓

Git commit

## Repository Truth Principle

Before any operation, the Operator must build a repository index from the current filesystem.

The repository index must include:

- `01 - 题目复盘/`
- `02 - 知识节点/`
- `00 - 每日复盘/`
- `06 - 二刷清单/`
- `90 - 原始材料/`

The current repository state is the source of truth for existing files.
Chat history is not a complete source of truth.
## Knowledge Base State Sync

After completing any of the following repository updates, the Obsidian Repository Operator must also update `.obsidian-ai/KNOWLEDGE_BASE_STATE.md`:

- Problem onboarding or problem review file updates.
- Daily review updates.
- Second-pass checklist updates.
- Knowledge node updates.

Sync requirements:

1. After adding a problem, update "已入库题目索引".
2. After adding an important problem, update "重点题目状态".
3. After adding a knowledge node, update "已有知识节点索引".
4. After updating the second-pass checklist, update "二刷与回炉清单摘要".
5. After adding a daily review, update "最近每日复盘摘要".
6. After adding a training issue, update "当前训练薄弱点".
