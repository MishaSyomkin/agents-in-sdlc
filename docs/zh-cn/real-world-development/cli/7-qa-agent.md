---
title: "第 7 课 - 创建并使用质量保证（QA）智能体"
description: "创建结合议题需求、quality-checks 技能和 Playwright 模型上下文协议（MCP）的质量保证（QA）自定义智能体。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个定义清晰的质量保证（QA）流程中。

在本课中，你将：

- 探索自定义智能体如何结合说明、技能和模型上下文协议（MCP）工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查其结果。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用角色。配置文件说明智能体的用途、指令和可用工具。本练习将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

本研讨会使用的自定义项各有不同用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 使用这些能力评估需求并报告结果。它们彼此配合，不需要另开对话。

## 创建 QA 配置

创建功能 PR 前，先请 Copilot 创建可复用的 QA 配置，定义其检查内容和行为边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 请默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先确认 Copilot 捕获了预期的 QA 流程和边界，避免只想验证功能时意外修改功能代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 为必需字段；`name` 是可选字段，但可提供清晰的显示名称。
3. 阅读说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或创建 PR。
5. 如果生成的配置遗漏上述职责或边界，请先要求默认智能体修订。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一筛选对话并选择 QA，以便它沿用已有的议题和计划上下文。

1. 使用以下提示词启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用列表中，但仍可使用上述命令启用。

2. 使用以下提示词请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体完成检查。
4. 阅读它生成的报告。

## 总结和后续步骤

你为工作流添加了可复用的专用角色，并审查了它的工作。在本课中，你：

- 探索了自定义智能体如何结合说明、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查了结果。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好审查。接下来，将它们整合到功能 PR 中，并使用 Agent Merge。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
