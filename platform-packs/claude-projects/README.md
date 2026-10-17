# ServiceNow Business Analyst — Claude Projects 配置说明

Claude Projects 支持上传知识文件 + 自定义指令，你的技能几乎可以**无损迁移**。

## 配置步骤

1. 新建 Project
2. 在「Project knowledge」中上传以下文件（直接拖入即可）：
   - 本目录 `knowledge/` 下的三份知识文档（methodology.md、platform-knowledge.md、elicitation-playbook.md）
   - 以及 `chatgpt-gpt/templates/` 下的 7 个交付物模板（或本目录 `templates/` 副本）
3. 在「Custom instructions」中粘贴 `instructions.md` 的内容
4. 保存即可，Claude 会在对话中自动引用上传的知识

## 文件清单

- `instructions.md` —— 项目自定义指令（角色 + 模块判定 + 工作流 + HRSD 三要素 + 原则）
- `knowledge/` —— 三份完整知识文档（原 skill 的 references/，平台无关的 Markdown）
- `templates/` —— 7 个交付物模板（原 skill 的 assets/templates/）

> 提示：Claude Projects 与 WorkBuddy 同为 Anthropic 系，SKILL.md 的正文（去掉 frontmatter 后）也可以直接作为项目说明上传，知识文档与模板无需任何改动。
