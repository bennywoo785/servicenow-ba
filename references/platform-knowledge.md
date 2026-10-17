# ServiceNow 平台知识基线（BA 必备，至少 CSA 管理员级理解）

BA 不需要写代码，但必须懂平台：干系人会随时提问，不懂配置层会在客户面前失分，也无法判断 OOTB vs 定制。

## 模块判定（先做：判断业务诉求属于哪个模块）

接到需求后，先按"业务诉求的对象"判定模块，再加载对应知识主线：

| 模块 | 业务对象 | 知识主线 |
|---|---|---|
| **HRSD** | 员工 / HR 事务（薪酬、福利、入转调离、证明、HR 服务） | HR Service / Lifecycle Event / Journey 三要素 |
| **ITSM** | IT 服务（事件、问题、变更、请求、服务目录） | Incident / Problem / Change / Request |
| **ITOM** | 基础设施监控与发现（告警、CMDB、Discovery） | 告警链路 / CI 与 Discovery |
| **CSM** | 外部客户服务（客户咨询、案例、客户门户） | Case / 客户服务目录 |
| 其他 | SAM/HAM（资产）、FSM（现场）、GRC（风控合规）等 | 对应模块定位 |

跨模块需求（如入职含 IT 设备申请 = HRSD + ITSM）→ 明确主模块与协同模块。无法判断 → 询问用户，不臆测。

## 模块版图

- **HRSD（人力资源服务交付）**：本技能重点领域，详见下方专章。核心三要素：**HR Service、Lifecycle Event、Journey**。
- **ITSM**（IT Service Management）：平台核心与最常见落地域，其他模块的表与流程多以 ITSM 为参照。覆盖事件（Incident）、问题（Problem）、变更（Change）、请求/服务目录（Request/Service Catalog）、知识管理（Knowledge）、SLA。
- **ITOM**（IT Operations Management）：基础设施监控与发现（Discovery/Event Management）。与 ITSM 的边界：ITSM 管服务的交付与维护流程，ITOM 管基础设施的发现与监控告警。
- **CSM**（客户服务管理）、**SAM/HAM**（软硬件资产管理）、**CMDB**、FSM、GRC 等。

## HRSD 深度知识（本技能重点领域）

### 核心三要素：HR Service / Lifecycle Event / Journey（必须背熟）

- **HR Service（HR 服务）= 服务定义层**：定义 HR 向员工提供什么服务（薪酬咨询、福利申请、证明开具等）。每个 HR Service 决定工单模板、字段、履约流程、SLA、可见性，挂在 COE 下。
- **Lifecycle Event（生命周期事件）= 事件触发层**：定义员工发生了什么变动——入职（Onboarding）、离职（Offboarding）、转岗（Transition）、晋升、产假等。事件一旦触发，自动启动对应流程。
- **Journey（员工旅程）= 体验编排层**：把 Lifecycle Event 触发后的所有工作编排成员工和履约方看到的路径——阶段（Stage）→ 活动（Activity）→ 任务（Task），跨 HR/IT/行政多部门协作。

**三者关系链（需求分析的主线）**：员工变动（如入职）→ 触发 Lifecycle Event → 生成 Journey（分阶段编排）→ 每个 Activity 派发 Task 给 HR/IT/行政履约 → 员工侧动作经由 HR Service / Employee Center 完成（如提交入职材料）。

### HRSD 高频混淆点（比 ITSM 混淆点更优先背熟）

1. **HR Service ≠ HR Case**：Service 是"定义"（模板、字段、流程、SLA、可见性），Case 是"实例"（某员工某一次的具体工单）。客户说"做一个入职服务"= 定义 HR Service，不是建一个 Case。
2. **Lifecycle Event ≠ Journey**：Lifecycle Event 是事件类型/触发器（员工发生了什么）；Journey 是该事件下员工实际走的路径（怎么走完）。Event 是"因"，Journey 是"过程"。Journey 是较新框架（Journey Designer / Journey Accelerator），旧版称 Enterprise Onboarding and Transitions。
3. **Activity ≠ Task**：Journey 的阶段（Stage）包含若干 Activity，Activity 落地为派发给履约方的 Task。编排用 Activity，干活用 Task。
4. **HR Service ≠ Service Catalog Item**：HR 服务用 HR Service 定义（挂 COE 下），不走 IT 的 Service Catalog；但员工在 Employee Center 看到的是统一入口。

### 其他关键对象

- **HR Case（HR 工单）**：HRSD 的核心工作对象，承载员工咨询与请求（薪酬、福利、证明开具、政策咨询等）。类比 ITSM 的 Incident/Request，但围绕"员工"而非"IT 服务"。
- **COE（Center of Excellence）数据模型**：HR 表按业务域分层组织（如薪酬福利 COE、员工关系 COE、人才管理 COE），HR Service 挂在 COE 下。需求梳理时先确认客户的 COE 划分，再挂服务。
- **HR Profile / Subject Person**：员工档案与工单关联的主体人；注意"工单打开人（Opened for）"与"主体人（Subject person）"可能不同（如 HR 代员工提单、经理为下属提单）。

### 关键体验与渠道

- **Employee Center（员工中心）**：统一员工门户，聚合 HR/IT 等多模块服务与知识；需求中涉及"员工自助"默认收敛到 Employee Center。
- **HR 知识库**：知识文章用 **User Criteria** 控制可见性（按部门/地区/员工类型）；HR 知识常与政策强绑定，需求中要写清内容维护责任方。
- **HR Agent Workspace**：HR 坐席工作台；涉及坐席效率的需求（列表配置、快捷回复、关联推荐）在此落地。
- **Now Mobile / 集成渠道**：移动端、邮件、Teams/企业微信等渠道接入。

### 保密性与权限模型（HRSD 与 ITSM 最大差异）

- HR 数据高度敏感：薪酬、纪律、医疗等信息必须严格隔离。HRSD 有独立的保密机制：**HR Criteria / User Criteria** 控制服务与知识的可见性，**安全组与角色**（如 sn_hr_core.* 系列角色）控制数据访问，敏感 Case 可标记 Confidential。
- BA 必须检查：任何 HRSD 需求都要回答"谁能看到这个工单/这条知识/这个字段？"——权限设计是 HRSD FRD 的必备章节，不能沿用 ITSM 的默认权限假设。

### 集成与文档

- **HCM 集成**：与 Workday、SAP SuccessFactors 等主数据系统集成（员工主数据、组织架构同步）——通常为 Inbound 到 ServiceNow；BA 需在 FRD 写清同步方向、字段映射、频率、差异处理。
- **Document Management / Document Templates**：员工文档管理与文档模板（在职证明等自动生成 PDF）。
- **电子签名（eSignature）**：与 DocuSign 等集成，常见于入职文件签署场景。

## ITSM 基础混淆点（次要，面试常考）

### 事件 / 问题 / 变更 的边界

- **Incident（事件）**：服务中断或质量下降的单个事件，目标是尽快恢复服务。例：Outlook 打不开。
- **Problem（问题）**：对反复发生或重大事件做**根因分析**，防止再发。例：Outlook 每隔两三天坏一次 → 立 Problem 查根因。
- **Change（变更）**：为修复问题或引入新能力而对生产环境做的**受控改动**，走审批流程。

> 常见错误：把"反复发生的事件做根因分析"说成变更管理——那是问题管理。

### ITSM vs ITOM

- ITSM：对已建成服务的维护与流程管理（类比：房子盖好后的物业维护）。
- ITOM：发现与监控基础设施状态（类比：楼宇监控发现哪台设备异常）。

### Inbound vs Outbound 集成

- **Inbound（入站）**：外部系统调用 ServiceNow（如外部系统 POST 到 SN 的 REST API 创建 Incident）。
- **Outbound（出站）**：ServiceNow 主动调用外部系统（如 SN 调用第三方接口同步数据）。

## CMDB 与 CI

- CMDB（配置管理数据库）存储 CI（配置项）：服务器、网络设备、应用等及其关系。
- Discovery 自动发现 CI 信息（服务器名、配置、网络、防火墙等），保持 CMDB 准确。
- BA 视角：需求涉及资产/依赖/影响分析时，先确认 CMDB 中 CI 数据是否可信。

## 集成三种方式

1. **CI/CD pipeline**（ServiceNow CI/CD、与 Jenkins 等流水线联动）——自动化发布/事件触发
2. **预建连接器（pre-built connectors / IntegrationHub spokes）**——优先选用，成本最低
3. **REST API**——最灵活但需开发；BA 需在 FRD 中写清入站/出站、认证方式、字段映射

典型场景链路示例：监控工具（Prometheus/告警引擎）设阈值（如 RAM>80%）→ 触发告警 → 经流水线/集成 → ServiceNow 自动建 Incident。

## 平台基础对象（BA 需能在 PDI 中导航演示）

- 表（Table）与表关系、表单（Form）与列表（List）、字段/变量（Variable）
- 用户 / 组 / 角色（User/Group/Role）与 ACL/RBAC 权限模型
- 服务目录（Service Catalog）与目录项（Catalog Item）全生命周期：提单 → 审批（可多级）→ 履约团队（fulfiller）→ SLA 计时 → 关闭
- Flow Designer 基础（低代码流程自动化；替代旧 Workflow 的首选）
- 通知（Notification）与审批（Approval）
- 报表与仪表盘（Report/Dashboard）、SLA 定义
- Update Set 概念（配置迁移单元，影响发布计划）

## OOTB 优先原则

需求评估顺序：**OOTB 功能 → 配置调整 → 定制代码**。FRD 中逐条标注用 OOTB 还是需要定制；定制必须说明理由与维护代价。收集需求时 BA 应先用 ServiceNow 语境引导客户（先演示已有目录项全生命周期，再问 SLA/审批层级/履约团队/变量）。

## 需求启发必备提问清单（服务目录类）

- SLA 目标（对照已有目录项，如 P4 = 8 天，新目录项要多少？）
- 审批层级数量与各级审批团队
- 履约团队（fulfiller）与工单路由规则
- 表单变量（用户视角的表单字段）
- 是否有区域/部门差异化要求
- 通知触发点与收件人
