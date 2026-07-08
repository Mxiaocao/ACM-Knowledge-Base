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