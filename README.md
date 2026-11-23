# ztocc-ui-check

一个面向已运行系统的 UI 体验走查与验收 Skill。它把“从人的角度用一遍”和“按标准逐项验收”放在同一个入口中，同时保持两类结论独立、可追溯。

## 能做什么

- **用户体验走查（playtest）**：以一个明确角色完成一项真实任务，记录第一人称过程、犹豫/误操作/反馈缺失、任务结果及证据。问题使用 `PT-###` 编号，不给正式验收结论。
- **正式 UI 验收（formal）**：对照已定稿的设计基准和验收矩阵检查声明范围，形成 `UI-###` 问题记录；只有前置条件齐全且指定验收人后，才能给出最终结论。
- **组合走查（combined）**：先做真实任务体验，再独立执行正式验收。体验严重度与验收 P0–P3 分级不会混为一谈。
- **自动生成测试场景**：用户不必先写角色和任务；Skill 会从页面路由、角色权限、业务流程、测试夹具和 E2E 测试中推导候选场景，并说明依据。
- **测试账号策略**：开始需角色账号的操作前，询问复用已标记的测试账号，还是在安全测试环境创建临时模拟账号并在结束后清理。用户已指定时不重复询问。
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
$ztocc-ui-check 根据这个项目的代码和测试夹具，自动选出一个核心角色与高价值场景，实际走查并汇报卡点。
```

```text
$ztocc-ui-check 对订单管理页面做正式 UI 验收，按验收矩阵检查并生成 HTML 报告。
```

```text
$ztocc-ui-check 先以仓管复核员身份完成昨日出库差异处理，再独立进行正式验收，输出一份合并 HTML 报告。
```

## 运行前准备

用户不必预先提供测试场景。没有指定角色/任务时，Skill 会先根据项目路由、权限模型、业务页面、fixtures/seeds 和现有 E2E 用例整理候选角色与任务，推荐一个高价值场景并给出来源依据。需要登录的角色走查会先询问账号策略：

- 复用明确标记为测试/演示用途的现有角色账号。
- 在已确认的本地、测试、预发或其他可丢弃环境创建临时模拟账号，走查后删除本轮创建的账号和数据。

用户已指定角色、任务或账号策略时按指定内容执行。若代码无法确认入口、角色、账号来源或安全清理方式，Skill 会指出具体缺口，不会猜凭据或直接写入。账号和业务数据的创建/删除仅限已确认的非生产可恢复环境，并且只清理本轮创建的资源；登录密码需由用户在浏览器手动输入，不能发到聊天中。创建账号不得向真实人员发送邀请、邮件或短信。

执行前的选择提示会列出推荐场景及代码依据、检测到的环境、可用账号/开通方式、计划写入和清理范围。无需登录的只读场景不会为了账号选择打断测试。

正式验收还需要：

定稿且带版本号的设计基准、明确页面范围、环境和数据、浏览器/设备范围、功能自测状态，以及负责最终判定的验收人。条件不齐时，Skill 会报告阻塞项，不会伪造“通过”。

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
- When no persona/task is supplied, derive grounded candidates from routes, permissions, business pages, fixtures and E2E tests. Before an authenticated role-based run, ask whether to reuse a designated test account or create a temporary synthetic account, unless the requester already chose.
- Also supports reacceptance, triage, report-only consolidation, and self-contained HTML reports.

Temporary accounts and test data may be created/deleted only in a confirmed non-production disposable environment, and cleanup is limited to resources created by the current run. Never use real employee/customer accounts as fixtures or request passwords in chat. See the Chinese sections above for details. No open-source license is currently declared.
