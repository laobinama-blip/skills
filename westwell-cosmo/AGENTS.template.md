# AGENTS.md (Westwell-Cosmo Template)

## Project Context

本仓库执行 `Westwell-Cosmo` 研发流程：

- OpenSpec：约束变更边界（SDD）
- Superpowers：编排执行流程（brainstorm/plan/execute/verify）
- TDD：实现前先写失败测试
- Hardness：上线前硬化闸门

## Mandatory Workflow

对任何业务行为变更、重构、接口改动、架构调整，必须按顺序执行：

0. Requirement Completeness Check
1. Exploration（Superpowers）
2. Spec Lock-In（OpenSpec）
3. Controlled Implementation（TDD）
4. Hardness Gate
5. Verify + Archive（OpenSpec）

`No gate pass, no next phase.`
`No output confirmation, no next action.`

## Phase Rules

### Phase 0: Requirement Completeness Check

- 必须先有完整需求包：目标、范围、非目标、约束、验收标准、边界条件。
- 需求不完整时，禁止进入设计和实现阶段。

Gate-0:

- 需求完整性检查通过。
- 输出物确认通过（需求包）。

### Phase 1: Exploration

- 先 `brainstorm` 再 `plan`。
- 禁止直接大面积改代码。
- 必须明确：目标、边界、非目标、验收标准、风险。

Gate-1:

- 团队确认推荐方案和风险控制策略。
- Phase 1 输出物逐项确认通过。

### Phase 2: Spec Lock-In

行为变更至少包含以下 OpenSpec 产物：

- `proposal.md`
- `design.md`
- `tasks.md`
- `delta spec`

Gate-2:

- 文档齐全、可执行、可评审，且需求方/负责人确认通过。
- 未确认不得进入实现阶段。

### Phase 3: Controlled Implementation

- 强制 TDD：`RED -> GREEN -> REFACTOR`。
- 小步提交，任务逐项实现和验证。
- PR 必须关联对应 OpenSpec 路径。

Gate-3:

- 代码、测试、规格三者一致。
- Phase 3 输出物逐项确认通过。

### Phase 4: Hardness Gate

合并前必须完成硬化检查：

1. Security：SAST/SCA 通过，高危问题清零或批准例外。
2. Stability：单测和集成测试通过。
3. Observability：日志、指标、链路追踪齐备。
4. Rollback：灰度和回滚策略明确可执行。
5. Compatibility/Performance：兼容与性能回归在阈值内。

Gate-4:

- 五项通过或存在已批准例外。
- Phase 4 输出物逐项确认通过。

## Output Confirmation Rule (Mandatory)

- 每次产出输出物后，先提交确认，再继续。
- 确认信息至少记录：输出物名称、版本/时间、确认人、确认结论。
- 未确认状态下，禁止自动推进到下一个动作或阶段。
- 输出物类型包含：文档、表格、图片附件、链接证据（CI/测试/安全扫描等）。

## Default Output List (All Confirmed)

- `PRD-<feature>.md`
- `AC-<feature>.md`
- `SPEC-<feature>.md`
- `Traceability-<feature>.xlsx`
- `TestCases-<feature>.md`
- CI 流水线链接
- `SecurityReport-<release>.md`
- `ReleasePlan-<release>.md`
- `Postmortem-<incident>.md`

如存在架构图/流程图/截图/录屏/PDF/原型图等附件，也必须进入确认清单。

### Phase 5: Verify + Archive

- verify 结论明确后才允许 archive。
- archive 前必须确认任务完成、测试通过、规格一致。

## Merge Requirements (CI Gate)

以下门禁未通过，禁止合并：

- Lint passed
- Unit tests passed
- Integration tests passed
- Coverage >= 80% (core modules >= 90% recommended)
- Build passed
- At least one non-author reviewer approved

## Classification Policy

### Small Changes

- 文案、日志、小重构可简化文档。
- 如影响行为，仍需 `delta spec`。

### Regular Features

- 必须先 `brainstorm`。
- 必须补齐 `proposal/design/tasks`。

### Complex Refactors

- 必须 `brainstorm` + 设计评审。
- 任务拆到最小可交付。
- verify 必须包含兼容与回滚检查。

## Branch and Delivery Conventions

- 优先使用独立工作区（worktree/feature branch）。
- 小步提交，避免跨模块无边界改动。
- 任何与规格冲突的改动先回到规格阶段更新。

## Review Checklist

Reviewer 至少验证：

1. 是否有对应 OpenSpec 路径和产物。
2. 是否遵守 TDD 并提供真实测试证据。
3. 是否通过 Hardness Gate。
4. 是否存在“代码先行、规格滞后”情况。
5. verify 结论是否充分，是否可归档。

## Operational Metrics

建议持续跟踪：

- AC 达成率
- 一次提测通过率
- CI 通过率与平均修复时长
- 线上缺陷率与变更失败率
- MTTR
- spec 覆盖率
- verify 覆盖率

## Exception Handling

- 紧急变更可走例外流程，但需记录审批、风险、回补计划。
- 例外仅影响时效，不豁免追溯与归档责任。

## Final Enforcement

`未过闸门，不进下一阶段。`

`规格、测试、代码不一致，不允许归档与合并。`
