# ServiceNow BA + Architect 技能集

一组专注于 **ServiceNow HRSD（人力资源服务交付）** 的 AI 技能包，覆盖「需求 → 设计 → 开发」端到端流程。

> **跨平台可用**：技能原生格式为 WorkBuddy Skill，同时提供主流 AI 平台的转换版本——ChatGPT（Custom GPT）、Claude Projects、Gemini Gems、Cursor、GitHub Copilot。核心知识、方法论与模板平台无关，仅外壳格式不同。

## 两个技能（构成流水线）

| 技能 | 角色 | 职责 | 产出 |
|---|---|---|---|
| **servicenow-ba** | 业务分析师 | 需求启发、BRD/FRD 编写 | BRD / FRD |
| **servicenow-architect** | 解决方案架构师 | FRD 拆解、技术方案设计 | 用户故事（Story+AC）/ TSD |

**流水线**：`servicenow-ba` 产出签核后的 FRD → `servicenow-architect` 承接，拆解为可开发的 Story + 技术设计文档（TSD），平台细节回退到官方文档（ServiceNowDocs）核查，默认 OOTB 优先、定制给理由与替代。

两个技能均全模块通用，**HRSD 为核心知识范围**（HR Service / Lifecycle Event / Journey 三要素，质量要求极高）。

## 工作流程（BA 第一步：模块判定）

接到需求后，**先判断业务诉求属于 ServiceNow 哪个模块**，再套用对应知识主线：

| 模块 | 业务对象 | 知识主线 |
|---|---|---|
| **HRSD** | 员工 / HR 事务 | HR Service / Lifecycle Event / Journey |
| **ITSM** | IT 服务 | Incident / Problem / Change / Request |
| **ITOM** | 基础设施监控与发现 | 告警链路 / CI 与 Discovery |
| **CSM** | 外部客户服务 | Case / 客户服务目录 |

本技能专精 **HRSD**，其余模块提供通用 BA 方法论 + 平台基线支撑。

## HRSD 核心知识主线（本技能重点）

以三要素为核心理解 HRSD 需求：

| 要素 | 分层 | 含义 |
|---|---|---|
| **HR Service** | 服务定义层 | HR 向员工提供什么（模板、字段、流程、SLA、可见性） |
| **Lifecycle Event** | 事件触发层 | 员工发生了什么变动（入职/离职/转岗/晋升） |
| **Journey** | 体验编排层 | 事件触发后员工实际走的路径（Stage → Activity → Task） |

**关系链**：员工变动 → 触发 Lifecycle Event → 生成 Journey → Activity 派发 Task 给 HR/IT/行政 → 员工经 HR Service / Employee Center 完成自助动作。

## 目录结构

```
servicenow-ba/
├── SKILL.md                          # 角色定位、双模式工作流、HRSD 知识基线入口
├── references/
│   ├── methodology.md                # 10 项职责、BRD vs FRD、敏捷双模式
│   ├── platform-knowledge.md         # HRSD 三要素核心 + 平台基线
│   └── elicitation-playbook.md       # 分级启发策略、Workshop 规范、HRSD 提问清单
├── assets/templates/                 # 7 个交付物模板
│   ├── brd-template.md
│   ├── frd-template.md               # 含 HRSD 保密性与可见性章节
│   ├── user-story-template.md
│   ├── rtm-template.md
│   ├── mom-template.md
│   ├── questionnaire-template.md
│   └── uat-plan-template.md
├── platform-packs/                   # 多平台转换包（跨平台用）
│   ├── chatgpt-gpt/                  #   ChatGPT Custom GPT
│   ├── claude-projects/              #   Claude Projects
│   ├── gemini-gem/                   #   Gemini Gems
│   ├── cursor/                       #   Cursor
│   ├── github-copilot/               #   GitHub Copilot
│   └── knowledge/                    #   平台无关的纯知识文档
├── servicenow-ba.zip                 # WorkBuddy 分发包
└── servicenow-ba-platform-packs.zip  # 多平台转换包（分发包）
```

## 安装

### WorkBuddy（原生）

1. 下载 `servicenow-ba.zip`
2. 解压到 `~/.workbuddy/skills/servicenow-ba/`（用户级）或项目的 `.workbuddy/skills/`（项目级）
3. 新会话中提出 ServiceNow HRSD / ITSM 需求类任务时会自动触发

### 其他 AI 平台（多平台转换包）

下载 `servicenow-ba-platform-packs.zip`（或直接用仓库内的 `platform-packs/` 目录），按平台取对应文件：

| 平台 | 机制 | 用什么 | 操作 |
|---|---|---|---|
| **ChatGPT**（Custom GPT） | Instructions + Knowledge | `chatgpt-gpt/instructions.md` + `templates/` | 新建 GPT → 粘贴指令 → 上传模板作知识 |
| **Claude Projects** | knowledge + custom instructions | `claude-projects/` 全套 | 上传知识/模板 + 粘贴指令（迁移最无损） |
| **Gemini Gems** | Instructions | `gemini-gem/instructions.md` | 新建 Gem → 粘贴指令 |
| **Cursor** | Rules | `cursor/.cursorrules` | 放项目根目录或粘贴到 Rules |
| **GitHub Copilot** | 指令文件 | `github-copilot/copilot-instructions.md` | 放 `.github/copilot-instructions.md` |
| **Windsurf** | `.windsurfrules` | `cursor/.cursorrules` 改名 | 内容通用，改文件名即可 |

> 核心内容（知识、方法论、模板）平台无关，转换只改外壳格式。`platform-packs/README.md` 内有完整说明。

## 使用示例

- "帮我梳理新员工入职（Onboarding）的需求"
- "给这个 HR 服务目录项写一份 FRD"
- "帮我设计员工转岗（Transition）的 Journey 活动编排"
- "组织一次 HR 需求收集 Workshop 并产出 MOM"
