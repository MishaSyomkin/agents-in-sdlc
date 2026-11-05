---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索管理上下文、选择模型、共享会话和可选委派任务的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供许多斜杠命令。这些命令提供高级功能、内部信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，接下来探索其他实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派任务到云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在来了解管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]等其他功能。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。如果想自行决定压缩时机，可以使用 `/compact`。切换到无关任务并需要全新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有不同偏好。Copilot CLI 可以列出模型并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验有助于大家更好地使用人工智能（AI）工具。`/share` 命令可将会话导出为 Markdown 或超文本标记语言（HTML）文件、创建共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先进行审查。会话导出可能包含提示词、回答和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或会话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已经结束。Copilot CLI 还为更大规模的任务提供以下命令：

- `/fleet` 可将彼此独立的子任务分配给子智能体并行运行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云智能体，让它异步工作并可能创建拉取请求。

这些命令是可选的，因为它们可能创建其他工作树或远程工作。尝试前先创建范围明确的新任务，并通过常规工作流审查结果。若要深入了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以进行配置、共享会话，并查看后台运行情况。在本课中，你：

- 使用 `/context` 和 `/compact` 查看了 Copilot 如何管理对话上下文。
- 使用 `/model` 探索了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和委派到云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索。接下来，[回顾所学内容][next-lesson]并了解后续学习资源。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
