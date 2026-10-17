# servicenow-ba

一个专注于 **ServiceNow HRSD（人力资源服务交付）** 的 Business Analyst 技能包（WorkBuddy Skill）。

扮演连接业务团队与 ServiceNow 平台团队的桥梁，处理 ServiceNow 项目的需求全生命周期：干系人识别、需求启发与 Workshop 组织、BRD/FRD 编写、用户故事与验收标准、流程映射（as-is/to-be）、差距分析、RTM、UAT 与缺陷管理、Go-Live 与 Hypercare。

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
└── assets/templates/                 # 7 个交付物模板
    ├── brd-template.md
    ├── frd-template.md               # 含 HRSD 保密性与可见性章节
    ├── user-story-template.md
    ├── rtm-template.md
    ├── mom-template.md
    ├── questionnaire-template.md
    └── uat-plan-template.md
```

## 安装

1. 下载 `servicenow-ba.zip`
2. 在 WorkBuddy 中解压到 `~/.workbuddy/skills/servicenow-ba/`（用户级）或项目的 `.workbuddy/skills/`（项目级）
3. 新会话中提出 ServiceNow HRSD / ITSM 需求类任务时会自动触发

## 使用示例

- "帮我梳理新员工入职（Onboarding）的需求"
- "给这个 HR 服务目录项写一份 FRD"
- "帮我设计员工转岗（Transition）的 Journey 活动编排"
- "组织一次 HR 需求收集 Workshop 并产出 MOM"
