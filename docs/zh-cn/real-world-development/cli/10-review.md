---
title: "第 10 课 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你通过 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。在此过程中，你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案、在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，自定义现有 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于上下文、模型、共享和可选云委派的斜杠命令。

## 已交付的内容

本研讨会包含两个 PR 阶段，每个阶段都基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化研讨会流程，这些工作合并到同一个 PR 中。

## 不同类型的验证

你通过自动化测试、浏览器手动检查和 Copilot 通过 MCP 进行的浏览器探索检查了代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按你新设定的格式报告结果。QA 汇总这些结果，并在 PR 创建前检查需求和测试覆盖范围。

新增测试应解决实际覆盖缺口；如果 QA 检查后确认无需添加测试，也可能是合理结果。合并前审查代码和证据；代码更改后，应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。说明定义标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不能只看智能体摘要。

根据任务选择**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理定义清晰的任务。日常修改可选择速度较快的模型，复杂任务则可选择能力更强的模型。

上下文仍与基础设施同样重要。明确描述希望构建的内容、原因和方式，会显著影响生成结果。

## 继续探索

核心工作流已完成，还可以了解一些 CLI 功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取另一种看法。
- `/fleet`：并行协调彼此独立的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法是持续实践。使用它编写生产代码、兴趣项目，或实现想了很久却一直没着手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想继续通过终端扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
