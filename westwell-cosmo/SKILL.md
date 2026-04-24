---
name: Westwell-Cosmo
description: Use when a team needs a governed AI delivery workflow that combines OpenSpec and Superpowers with SDD, TDD, and hardness gates from requirement to archive.
---

# Westwell-Cosmo

## Overview

`Westwell-Cosmo` 是把你现有两份文档合并后的统一研发技能：

- `OpenSpec` 负责 SDD（规格先行，约束边界）
- `Superpowers` 负责流程编排（brainstorm -> plan -> execute -> verify）
- `TDD` 负责实现质量（先测后码，红绿重构）
- `Hardness` 负责上线前硬化闸门（安全、稳定、可观测、可回滚、兼容）

一句话：`先定边界，再做实现；先过闸门，再进下一步`。

## Skill Composition

`Westwell-Cosmo` 作为组合技能，显式包含两类能力：

- `superpowers` 能力：
  - `using-superpowers`
  - `brainstorming`
  - `writing-plans`
  - `executing-plans`
  - `test-driven-development`
  - `verification-before-completion`
- `openspec` 能力：
  - 核心流：`/opsx:propose` ` /opsx:apply` `/opsx:archive`
  - 扩展流：`/opsx:new` `/opsx:continue` `/opsx:ff` `/opsx:verify`
  - 治理补充：`/opsx:explore` `/opsx:sync` `/opsx:bulk-archive` `/opsx:onboard`

## When To Use

适用于以下场景：

- 任何技术栈团队使用 AI 开发（后端/前端/移动端/数据与AI/平台与基础设施）
- 需求经常出现“先改代码后补规格”的混乱
- 需要把需求、设计、实现、测试、验证、归档串成统一流水线
- 希望团队统一 AI 研发动作和门禁标准

不适用于：

- 一次性 PoC 或临时脚本（可走简化审批）

## Mandatory Rules

1. 代码实现前，必须先完成探索与规格锁定。
2. 前期必须有完整需求；需求不完整时，仅允许补充需求，不进入设计与实现。
3. 每一个输出物（含文档、表格、图片附件、链接证据）都必须由需求方/负责人确认通过后，才可进入下一步。
4. 没有 `OpenSpec` 产物（至少 proposal/tasks），不进入实现。
5. 没有真实测试结果，不宣称完成。
6. 代码、测试、规格不一致，不允许归档。
7. 影响业务行为的变更，必须有 `delta spec`。

## Engineering Principles (From R&D Spec)

1. 需求可追踪：
   - 每个需求必须映射到 AC、tasks、tests、PR、release 记录。
2. 质量前置：
   - 未通过测试与门禁不得合并。
3. 安全默认开启：
   - 未通过安全基线不得发布。
4. 小步快跑：
   - 小批次交付，支持快速回滚。
5. 人工兜底：
   - AI 可生成内容，但责任人必须人工评审并签字。

## Confirmation Protocol (Mandatory)

- 阶段内产生输出物后，必须先“提交确认”，再进入下一动作。
- 未确认状态下，流程必须暂停，不得自动继续。
- 确认记录至少包含：输出物清单、版本/时间、确认人、确认结论（通过/退回）。

## Output Catalog (All Must Be Confirmed)

以下输出物默认纳入确认范围（按你提供的模板清单）：

1. 需求：`PRD-<feature>.md`（PO/PM）
2. 验收标准：`AC-<feature>.md`（PO/PM、QA）
3. 规格设计：`SPEC-<feature>.md`（TL）
4. 追踪矩阵：`Traceability-<feature>.xlsx`（TL、QA）
5. 测试用例：`TestCases-<feature>.md`（QA、Dev）
6. CI 结果：CI 流水线链接（DevOps）
7. 安全报告：`SecurityReport-<release>.md`（AppSec）
8. 发布计划：`ReleasePlan-<release>.md`（PO）
9. 复盘报告：`Postmortem-<incident>.md`（PO、TL）

补充规则：

- 若阶段内有架构图、流程图、截图、录屏、PDF、原型图等附件，这些附件与其链接同样属于“必须确认输出物”。
- 输出物与附件需在同一确认单中逐项标记状态（通过/退回/待补充）。

## RACI (Recommended)

- PO/PM：对需求、范围、验收标准与发布计划负责。
- TL：对技术方案、分层设计、风险控制与规格一致性负责。
- Dev：按任务实现与测试，保证代码与规格对齐。
- QA：对测试用例、验收验证与质量结论负责。
- DevOps：对 CI、部署流水线、发布可执行性负责。
- AppSec：对安全扫描、漏洞处置与例外审批意见负责。
- Reviewer（非提交者）：对合并质量门禁负责。

## Governance Architecture

推荐采用“双层仓库 + 三层规则”：

- 双层仓库：
  - 中央规范仓库：沉淀组织级标准（API、安全、审计、鉴权、网关、日志）。
  - 服务本地 OpenSpec：维护服务基线规格与本次增量变更。
- 三层规则：
  - 全局规则：`~/.codex/AGENTS.md`
  - 项目规则：`<repo>/AGENTS.md`
  - 规格规则：`openspec/specs` 与 `openspec/changes`

## Codex Operating Notes

- `AGENTS.md` 分层生效：就近目录规则优先级更高。
- Skills 可手动调用，也可按任务自动匹配。
- 建议在仓库内显式写入 `Westwell-Cosmo` 强制流程，避免执行偏差。

## Workflow

### Phase 0: 需求分级

先分级，再决定流程强度：

- 小改动（文案/日志/微重构）：
  - 可简化文档
  - 若影响行为，必须补 `delta spec`
- 常规功能（新增接口/流程调整）：
  - 必须 `brainstorm`
  - 必须 `proposal + design + tasks`
- 复杂重构（拆模块/改鉴权/换库）：
  - 必须 `brainstorm`
  - 必须设计评审
  - 必须最小可交付拆分
  - `verify` 必须含回滚与兼容检查

新增前置要求（适用于全部等级）：

- 必须先形成完整需求包（建议最少包含：目标、范围、非目标、约束、验收标准、边界条件）。
- 若需求包不完整，流程停在 Phase 0，只做需求补齐。

Gate-0（通过后进入 Phase 1）：

- 需求完整性检查通过。
- 需求包确认通过（目标/范围/非目标/约束/验收标准/边界条件）。

### Phase 1: Superpowers 探索规划（禁止写代码）

先调用：

- `using-superpowers`
- `brainstorming`
- `writing-plans`

输出物：

- 问题定义（目标/边界/非目标）
- 2-3 个候选方案与取舍
- 风险清单与测试思路
- 可评审的设计草稿

Gate-1（通过后进入 Phase 2）：

- 需求边界清晰
- 团队确认推荐方案
- 风险与验收标准明确
- Phase 1 全部输出物确认通过

### Phase 2: OpenSpec 锁定规范（SDD）

调用 `openspec`，按项目配置选择核心或扩展命令流：

- 核心：`/opsx:propose -> /opsx:apply -> /opsx:archive`
- 扩展：`/opsx:new -> (/opsx:ff | /opsx:continue) -> /opsx:apply -> /opsx:verify -> /opsx:archive`

最低产物要求：

- `proposal.md`（为什么做、做什么、影响范围）
- `design.md`（技术方案、替代方案、风险）
- `tasks.md`（可执行任务清单）
- `delta spec`（行为变更边界）

扩展命令建议（按需要使用）：

- 需求尚不清晰时：`/opsx:explore`
- 多变更合并基线时：`/opsx:sync`
- 批量归档场景：`/opsx:bulk-archive`
- 新成员接入：`/opsx:onboard`

Gate-2（通过后进入 Phase 3）：

- OpenSpec 文档齐全且可执行
- Phase 2 全部输出物确认通过
- 未确认则停留在 Phase 2，不进入实现

### Phase 3: 受控实现（TDD + Plan Execution）

先调用：

- `executing-plans`
- `test-driven-development`

执行要求：

1. 先测后码（RED -> GREEN -> REFACTOR）
2. 小步提交，避免大面积无边界改动
3. 每项任务完成后立即验证
4. PR 必须关联 OpenSpec 路径

Gate-3（通过后进入 Phase 4）：

- tasks 完成度达标
- 测试通过且与规格一致
- 变更可回溯到对应 spec/tasks
- Phase 3 全部输出物确认通过

### Phase 4: Hardness Gate（硬化闸门）

`Hardness` 在本技能中定义为上线前的五类硬化：

1. Security：
   - SAST/SCA 通过
   - 高危漏洞清零或有批准例外
2. Stability：
   - 单测/集成测试通过
   - 核心路径无阻断缺陷
3. Observability：
   - 日志、指标、链路追踪齐备
   - 告警与排障入口可用
4. Rollback：
   - 灰度策略明确
   - 回滚步骤与触发条件明确
5. Compatibility & Performance：
   - 兼容性检查通过
   - 性能回归在可接受范围

建议同时调用：

- `verification-before-completion`
- `security-best-practices`（按技术栈执行）

Gate-4（通过后进入 Phase 5）：

- 五类硬化检查均通过或有批准例外
- Phase 4 全部输出物确认通过

## CI Quality Gate (Mandatory)

未满足以下门禁，禁止合并：

1. Lint 通过
2. 单元测试通过
3. 集成测试通过
4. 覆盖率 >= 80%（核心模块建议 >= 90%）
5. 构建成功
6. 至少 1 名非提交者评审通过

## Security / Gray Release / Observability / Exceptions

- 安全治理：SAST/SCA 必须执行，高危问题需清零或批准例外。
- 灰度发布：明确放量策略、回滚条件、回滚路径。
- 可观测性：日志、指标、链路追踪齐备，告警入口可用。
- 变更例外：可走审批，但必须记录风险、责任人、回补计划。

### Phase 5: Verify + Archive + 交付

调用：

- `openspec` 的 verify / archive 流程

完成标准：

- 规格、代码、测试三者一致
- 验证结论清晰可追溯
- OpenSpec 归档完成
- PR 合并后进入发布与观测

## KPI

建议跟踪以下指标：

- 需求 AC 达成率
- 一次提测通过率
- CI 通过率与平均修复时长
- 线上缺陷率 / 变更失败率
- MTTR
- 有 spec 的需求占比
- verify 覆盖率
- PR 平均改动行数

## Installation & Initialization

Superpowers（Codex）：

```powershell
git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\\.codex\\superpowers"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\\.agents\\skills"
cmd /c mklink /J "$env:USERPROFILE\\.agents\\skills\\superpowers" "$env:USERPROFILE\\.codex\\superpowers\\skills"
```

OpenSpec：

```bash
npm install -g @fission-ai/openspec@latest
openspec init
openspec config profile
openspec update
```

## Rollout Cadence (Recommended)

- 阶段一（1周，个人试点）：
  - 跑通 1 个中等需求全流程。
- 阶段二（2-4周，服务级试点）：
  - 按需求分级执行，沉淀模板与检查清单。
- 阶段三（1-2个月，组织推广）：
  - 建中央规范仓库，统一准入、评审与验收标准。

四周稳步推进（可执行版）：

1. 第1周：试点
2. 第2周：模板化（探索模板、OpenSpec artifact 模板、实现计划、Review checklist）
3. 第3周：写入团队规范（`AGENTS.md` / `CONTRIBUTING.md` / PR 模板）
4. 第4周：推广（先覆盖非平凡 feature，再收反馈精简流程）

## Team Adoption Checklist

- 平台团队：
  - 安装并维护 Codex/OpenSpec/Superpowers
  - 提供全局 `AGENTS.md`、项目 `AGENTS.md`、OpenSpec 模板
  - 提供 Prompt 模板、验证清单、合并门禁基线
- 服务团队：
  - 初始化项目 OpenSpec
  - 补齐服务基线 specs
  - 新需求按流程产出并关联 spec
- 开发者：
  - 进入仓库先确认 `AGENTS.md`
  - 复杂任务先 `brainstorm`，不直接大面积改代码
  - 先 plan 再 execute，结束前做验证

## Architecture Layering Reference (Multi-Stack)

本技能不限制语言/框架，建议采用“职责分层”而非“框架绑定”：

- Interface Layer：
  - API/UI/CLI 入口，负责输入输出边界与协议适配。
- Application Layer：
  - 用例编排、流程控制、事务边界。
- Domain Layer：
  - 核心业务规则、领域模型、约束。
- Infrastructure Layer：
  - 数据存储、消息系统、第三方集成、运维侧能力。

多栈落地示例（可选）：

- Java/Spring、.NET、Node.js、Python、Go：按上述四层映射到各自项目结构。
- 前端（React/Vue/Angular）：
  - 页面与组件（Interface）/ 状态与用例（Application）/ 领域规则（Domain）/ API与存储适配（Infrastructure）。
- 数据与AI工程：
  - 任务入口（Interface）/ pipeline 编排（Application）/ 特征与业务规则（Domain）/ 训练与推理基础设施（Infrastructure）。

## Common Misconceptions

- “Superpowers 会改业务代码”：
  - 不会。它改变的是执行流程，不是业务语义。
- “OpenSpec 会拖慢开发”：
  - 小改动可轻量，复杂需求走完整流程可显著减少返工。
- “有 Codex 就不需要治理”：
  - AI 能力越强，越需要明确边界与门禁。
- “中央规范可替代本地规格”：
  - 不可替代。中央管标准，本地管服务边界。

## Communication Phrases

- “先做探索，不直接改代码。”
- “先补 OpenSpec，再进入实现。”
- “这个输出物请先确认，通过后继续。”
- “没有 verify 结论，不进入归档/合并。”

## Quick Start

1. 在仓库确认 `AGENTS.md` 和 OpenSpec 初始化状态。
2. 按需求分级进入对应流程强度。
3. 严格执行五阶段并卡闸门。
4. 任何阶段闸门未通过，回退到上一阶段补齐。

## Team Assets

- 团队一页 SOP：`TEAM-SOP.md`
- 项目 AGENTS 完整模板：`AGENTS.template.md`
- 输出物确认清单：`OUTPUT-CONFIRMATION-CHECKLIST.md`

## AGENTS.md Snippet (Recommended)

```md
## Westwell-Cosmo Workflow (Mandatory)

For any behavior-changing feature, refactor, or integration:
1) Superpowers Exploration First (brainstorm + plan), no coding.
2) OpenSpec Lock-In (proposal/design/tasks/delta spec).
3) Controlled Implementation with TDD.
4) Hardness Gate before completion (security/stability/observability/rollback/compatibility).
5) OpenSpec verify + archive, then merge.

No gate pass, no next phase.
```
