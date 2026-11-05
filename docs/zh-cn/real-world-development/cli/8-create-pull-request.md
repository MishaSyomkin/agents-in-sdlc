---
title: "第 8 课 - 创建并合并筛选功能拉取请求"
description: "一起审查筛选功能、说明、技能更新、质量保证（QA）配置和测试，然后创建拉取请求（PR）并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一分支上。现在一起审查这些更改并创建拉取请求（PR）。星级评分 PR 是你手动合并的；这次将由 **Agent Merge** 管理合并流程。

> [!NOTE]
> 实际工作中，通常会将功能、说明、技能和 QA 智能体拆分到几个 PR 中。为简化本研讨会流程，这些工作共用一个对话和分支，并放在同一个 PR 中。

在本课中，你将：

- 了解 Agent Merge 及其如何自动化合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动处理剩余 PR 工作，同时让开发人员掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并 PR 所需的后续工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时进行变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

之前你都自行选择 **Merge pull request**。启用 Agent Merge 前，仍需先审查并确认更改已准备就绪。

## 使用 Agent Merge 管理 PR

代码完成后，一起审查代码、创建 PR，并让 Agent Merge 处理其余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用 `/pr create` 创建 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程；所有检查成功后，它会执行合并。
7. 确认收到类似 `PR #14 was squash-merged successfully.` 的 Copilot 消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果操作受阻，请查看报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程中的多个部分，包括代码生成、测试和验证，以及拉取请求处理。在本课中，你：

- 了解了 Agent Merge 如何自动化合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来，探索更多实用的 Copilot CLI 斜杠命令。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
