# ztocc-ui-check

一个面向已运行系统的 UI 体验走查与验收 Skill。它把“从人的角度用一遍”和“按标准逐项验收”放在同一个入口中，同时保持两类结论独立、可追溯。

## 能做什么

- **用户体验走查（playtest）**：以一个明确角色完成一项真实任务，记录第一人称过程、犹豫/误操作/反馈缺失、任务结果及证据。问题使用 `PT-###` 编号，不给正式验收结论。
- **正式 UI 验收（formal）**：对照已定稿的设计基准和验收矩阵检查声明范围，形成 `UI-###` 问题记录；只有前置条件齐全且指定验收人后，才能给出最终结论。
- **组合走查（combined）**：先做真实任务体验，再独立执行正式验收。体验严重度与验收 P0–P3 分级不会混为一谈。
- **复验、问题分诊和报告整理**：支持修复后复验、基于截图/录屏的问题分诊，以及整理已有发现。
- **HTML 报告**：按任务生成单文件 HTML 报告，保留范围、结论、问题记录及相对路径证据。

## 安装

克隆到 Codex 的个人 skills 目录：

```bash
git clone https://github.com/houpe/ztocc-ui-check.git ~/.codex/skills/ztocc-ui-check
```

也可以把目标路径改为项目内的 `.agents/skills/ztocc-ui-check/`，用于仅在该项目中启用。

## 使用示例

```text
$ztocc-ui-check 扮演仓库操作员，完成“把今天到货的一批冷藏品入库”，记录真实体验和卡点。
```

```text
$ztocc-ui-check 对订单管理页面做正式 UI 验收，按验收矩阵检查并生成 HTML 报告。
```

```text
$ztocc-ui-check 先以仓管复核员身份完成昨日出库差异处理，再独立进行正式验收，输出一份合并 HTML 报告。
```

## 运行前准备

用户体验走查需要明确：

1. 一个具体角色及其工作环境、目标和成功标准。
2. 一项有明确完成条件的任务。
3. 可访问的系统入口和经授权的登录方式。
4. 环境、测试数据、允许的写入/破坏性操作和排除范围。

正式验收还需要定稿且带版本号的设计基准、明确页面范围、环境和数据、浏览器/设备范围、功能自测状态，以及负责最终判定的验收人。条件不齐时，Skill 会报告阻塞项，不会伪造“通过”。

推荐在项目中提供可复用的非敏感配置：

```text
.playtest/
├── project.md
└── personas/
    └── warehouse-operator.md
```

不要把密码、Token、Cookie、私钥或生产敏感数据写入配置、截图或报告。涉及生产写入、删除、付款、派单等外部副作用时，必须先明确授权并使用可恢复/可丢弃的数据。

## 目录结构

```text
SKILL.md
README.md
references/
├── acceptance-matrix.md
├── reporting.md
├── playtest-playbook.md
└── playtest-reporting.md
```

## 结论边界

- `PT-###` 是体验摩擦记录；初步高/中/低严重度不等于正式缺陷等级。
- `UI-###` 是正式验收问题；最终分级和通过/不通过由指定验收人负责。
- 未实际操作的内容必须标记为未覆盖或推测；静态截图不能证明交互行为。
- `ui-ux-pro-max` 和 `impeccable` 可作为独立诊断/改进参考，不替代验收基准和证据。

## License

当前仓库尚未声明开源许可证。公开可见不代表授予复制、修改或再分发许可。

---

# English

`ztocc-ui-check` is a Codex skill for evaluating an already-running product from both a human task perspective and a formal UI acceptance perspective, without conflating their findings.

- `playtest`: complete one concrete task as one persona; record a first-person journey, friction (`PT-###`), outcome, and evidence. It does not issue a formal verdict.
- `formal`: check a declared scope against a versioned design baseline and acceptance matrix. A final verdict requires complete prerequisites and a named acceptor.
- `combined`: run playtesting first, then perform formal acceptance independently. Experience severity is not the same as P0-P3.
- Also supports reacceptance, triage, report-only consolidation, and self-contained HTML reports.

See the Chinese sections above for installation, examples, prerequisites, safety boundaries, and repository layout. Do not store credentials or sensitive production data in project configuration or reports. No open-source license is currently declared.
