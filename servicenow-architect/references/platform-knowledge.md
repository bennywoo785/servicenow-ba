# ServiceNow 平台知识基线（架构师视角）

架构师比 BA 需要更深的平台理解：不仅要懂"有什么"，还要懂"怎么配置/怎么定制、定制点在哪、代价是什么"。本文件是技术方案的决策依据，设计前必须内化。

## 模块判定（先做，与 BA 对齐）

| 模块 | 业务对象 | 技术主线 |
|---|---|---|
| **HRSD**（核心领域） | 员工/HR 事务 | HR Service / Lifecycle Event / Journey + 保密性权限 |
| **ITSM** | IT 服务 | Incident / Problem / Change / Request + 服务目录 |
| **ITOM** | 基础设施监控与发现 | 告警链路 / CMDB / Discovery |
| **CSM** | 外部客户服务 | Case / 客户门户 / 客户服务目录 |
| 其他 | SAM/HAM（资产）、FSM（现场）、GRC（风控） | 对应模块 |

## 方案决策基线：OOTB → 配置 → 定制

- **OOTB**：平台原生功能直接可用，默认首选。
- **配置**：不改代码，通过表单设计、字典、业务流程、Flow Designer、ACL 规则、SLA 定义等满足。次选。
- **定制**：需写脚本或代码。最后手段，必须附理由 + 维护代价 + 替代方案。

### 常见定制点（架构师必须清楚"在哪里定制"）

| 定制点 | 用途 | 代价/风险 |
|---|---|---|
| Business Rule | 服务端逻辑（增删改查前/后触发） | 影响性能、升级需回归 |
| Client Script | 表单客户端交互/校验 | 只作用于表单，接口旁路 |
| Script Include | 可复用服务端函数（被 BR/Flow/API 调用） | 需单元测试、文档 |
| UI Action / UI Policy | 按钮、字段动态控制 | 配置优先，少用脚本 |
| Flow Designer 扩展 | 自定义流程动作（Action/Subflow） | 相对可控，优先于 BR |
| Scheduled Job | 定时任务 | 需监控失败、超时 |
| ACL / 脚本 ACL | 精细权限 | 安全敏感，需审计 |
| 集成开发 | REST/SOAP/自定义 Spoke | 需认证、字段映射、错误处理 |

> 决策铁律：能用配置就不写脚本；能用 Flow Designer 就不写 BR；必须写脚本就收敛到 Script Include 复用。

## HRSD 深度知识（核心领域，要求极高掌握度）

### 核心三要素（架构师必须精确到配置层）

- **HR Service（服务定义层）**：挂在 COE 下的服务定义，决定工单模板、字段、履约流程、SLA、可见性。架构师要落到具体配置：HR Service 表（sn_hr_core_service）、关联的 case 表、字段与可见性规则。
- **Lifecycle Event（事件触发层）**：员工变动（入职/离职/转岗/晋升）事件，触发后启动 Journey。架构师要设计触发条件（HCM 状态变更 or 人工发起）。
- **Journey（体验编排层）**：Stage → Activity → Task 的编排。架构师要设计每阶段的活动、跨部门派发、超时/升级、可自动化项。Journey 是较新框架（Journey Designer / Journey Accelerator）。

**关系链（技术方案主线）**：员工变动 → 触发 Lifecycle Event → 生成 Journey → Activity 派发 Task 给 HR/IT/行政 → 员工经 HR Service / Employee Center 完成自助动作。

### 四组高频混淆点（设计时防止用错对象）

1. **HR Service ≠ HR Case**：Service 是定义，Case 是实例。设计"入职服务"= 定义 HR Service。
2. **Lifecycle Event ≠ Journey**：Event 是触发器（因），Journey 是过程编排（过程）。
3. **Activity ≠ Task**：编排用 Activity，履约用 Task（派发给履约方）。
4. **HR Service ≠ Service Catalog Item**：HR 服务挂 COE 下，不走 IT 服务目录；Employee Center 统一入口。

### 保密性与权限模型（HRSD 技术设计必做）

- **HR Criteria / User Criteria**：控制服务与知识的可见性（按部门/地区/员工类型）。
- **HR 安全组与角色**：`sn_hr_core.*` 系列角色控制数据访问。
- **Confidential 标记**：敏感 Case 标记 Confidential，进一步限制访问。
- 架构师必须**逐对象**（Case/知识/字段）设计可见范围，禁止沿用 ITSM 默认权限假设。
- 定制的 ACL/脚本 ACL 要审计，避免破坏 OOTB 保密模型。

### 其他关键对象与配置点

- **COE 数据模型**：HR 表按业务域分层（薪酬福利/员工关系/人才管理），HR Service 挂 COE 下。
- **Subject Person vs Opened for**：代提单（经理代下属、HR 代员工）时两者分离，OOTB 支持，勿自定义。
- **Employee Center**：员工统一门户，自助服务收敛点。
- **HR Agent Workspace**：坐席工作台，列表配置、快捷回复、关联推荐。
- **HCM 集成**：Workday/SuccessFactors 等主数据同步，通常 Inbound；设计同步方向、字段映射、频率、差异处理。
- **Document Template / eSignature**：文档模板生成 PDF（在职证明），DocuSign 电子签署（入职文件）。

## ITSM / ITOM / 其他模块要点（通用支撑）

### ITSM

- **Incident**（单次中断，尽快恢复）/ **Problem**（反复事件根因分析）/ **Change**（受控改动走审批）。
- 服务目录项全生命周期：提单 → 审批（可多级）→ 履约团队 → SLA 计时 → 关闭。
- 架构师关注：目录项的变量设计、审批流（Flow/Approval）、fulfiller 路由、SLA 定义。

### ITOM

- 告警链路：监控工具阈值 → 告警 → 集成 → 自动建 Incident。
- CMDB/CI：配置项（服务器/网络/应用）及关系；Discovery 自动发现保持准确。
- 架构师关注：CI 数据可信度、告警到 Incident 的映射规则。

### CSM

- Case 管理、客户门户、客户服务目录；围绕"外部客户"而非"员工/IT"。

## 集成三种方式（技术设计必考）

1. **CI/CD pipeline**（ServiceNow CI/CD 联动 Jenkins 等）——自动化发布/事件触发
2. **预建连接器（IntegrationHub Spoke）**——优先选用，成本最低；先查有没有现成 Spoke
3. **REST API**——最灵活但需开发；设计入站/出站、认证（OAuth/Basic）、字段映射、错误处理

**Inbound**（外部调用 SN）vs **Outbound**（SN 调用外部）。HCM 集成通常为 Inbound 到 SN。

## 平台基础对象（架构师需能在 PDI 演示并定位配置）

- 表（Table）与表关系、扩展表继承；字段类型（string/integer/reference/choice/list/date 等）
- 表单（Form）与列表（List）、变量（Variable）与变量集（Variable Set）
- 用户/组/角色（User/Group/Role）与 ACL/RBAC
- Flow Designer（低代码流程自动化，替代旧 Workflow）
- 通知（Notification）与审批（Approval）
- 报表与仪表盘（Report/Dashboard）、SLA 定义与指标
- Update Set 概念（配置迁移单元，影响发布计划与升级）

## 官方文档核查（ServiceNowDocs）

设计涉及具体字段/配置/API/版本时，回退官方文档，禁止臆测：
- 仓库：`https://github.com/ServiceNow/ServiceNowDocs`
- 入口：`https://raw.githubusercontent.com/ServiceNow/ServiceNowDocs/brazil/llms.txt`
- 版本分支：brazil（最新）/ australia / zurich / yokohama
- Raw URL：`.../markdown/{publication}/{file}`
- 勿抓 `servicenow.com/docs`（JS 单页应用，LLM 读不出）
