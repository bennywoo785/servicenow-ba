# ServiceNow BA 多平台转换包

把 `servicenow-ba` 技能（HRSD 专精的 ServiceNow 业务分析师）转换为**主流 AI 平台可用的格式**。

> 核心内容（知识、方法论、模板）是**平台无关的**，转换只改外壳格式。

## 目录结构与对应平台

```
platform-packs/
├── chatgpt-gpt/                    # ChatGPT Custom GPT
│   ├── instructions.md             #   粘贴到 Instructions
│   └── templates/                  #   7 个模板，上传到 Knowledge
│
├── claude-projects/                # Claude Projects（Anthropic，迁移最无损）
│   ├── README.md                   #   配置步骤说明
│   ├── instructions.md             #   粘贴到 Custom instructions
│   ├── knowledge/                  #   三份知识文档，上传 Project knowledge
│   └── templates/                  #   7 个模板，上传 Project knowledge
│
├── gemini-gem/
│   └── instructions.md             #   粘贴到 Gemini Gems 的 Instructions
│
├── cursor/
│   └── .cursorrules                #   项目根目录或 Cursor Rules
│
├── github-copilot/
│   └── copilot-instructions.md     #   放 .github/copilot-instructions.md
│
├── knowledge/                      # 平台无关的纯知识文档（通用素材）
│   ├── methodology.md
│   ├── platform-knowledge.md
│   └── elicitation-playbook.md
│
└── others/                         # 预留（如 Windsurf、DeepSeek 等）
```

## 各平台快速指引

| 平台 | 机制 | 用什么文件 | 操作 |
|---|---|---|---|
| **ChatGPT (Custom GPT)** | Instructions + Knowledge | `chatgpt-gpt/instructions.md` + `templates/` | 新建 GPT → 粘贴指令 → 上传模板作知识 |
| **Claude Projects** | Project knowledge + custom instructions | `claude-projects/` 全部 | 上传知识/模板 + 粘贴指令 |
| **Gemini Gems** | Instructions | `gemini-gem/instructions.md` | 新建 Gem → 粘贴指令 |
| **Cursor** | Rules | `cursor/.cursorrules` | 放项目根目录或粘贴到 Rules |
| **GitHub Copilot** | 指令文件 | `github-copilot/copilot-instructions.md` | 放 `.github/copilot-instructions.md` |
| **Windsurf** | `.windsurfrules` | `cursor/.cursorrules` 改名 | 内容通用，改文件名即可 |

## 注意事项

1. **模板是通用素材**：7 个交付物模板（BRD/FRD/用户故事/MOM/问卷/RTM/UAT）在 `chatgpt-gpt/templates/` 和 `claude-projects/templates/` 各有一份，也可从 `knowledge/` 体系里复用。
2. **规则类平台（Cursor/Copilot/Windsurf）**：已把核心指令压缩成紧凑规则文件，牺牲少量展开说明以适配规则文件长度限制。
3. **知识文档通用**：`knowledge/` 下的三份文档是纯 Markdown，任何支持知识库的平台（Notion AI、DeepSeek 等）都能直接上传。
