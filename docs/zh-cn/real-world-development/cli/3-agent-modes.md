---
title: "第 3 课 - 智能体模式：计划和自动驾驶"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的开发流程，帮助我们以正确的方式构建正确的功能。本课将开启一系列课程，带你体验典型的人工智能（AI）智能体驱动开发流程：从议题着手生成新功能，验证代码和功能行为，最后将更改成功合并到项目中。

> [!NOTE]
> 继续实现此功能时，将沿用同一对话和分支。实际工作中，通常会为不同类型的文件创建不同分支或拉取请求（PR）；本研讨会为聚焦核心概念而简化了流程。

在本课中，你将：

- 根据 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加模型上下文协议（MCP）验证、创建质量保证（QA）智能体并创建功能拉取请求。

## 场景

Tailspin Toys 的游戏目录不断扩充，访客需要按类别和发行商筛选游戏。待办议题描述了此功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式商定这些决定，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体，并不会改变软件开发的基本原则。通常的工作流程如下：

1. 从描述需求的已创建议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这一标准流程，可以确保人工智能（AI）生成的代码符合要求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并反复迭代，直到认为任务完成。

先使用 Plan 模式审查计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示词并返回 **Session** 选项卡。

现在提示词中应包含 `#7` 或类似编号。`#` 可将 GitHub 议题或拉取请求（PR）加入上下文。

## 规划筛选功能

在实现功能前制定计划，可以先确定实现方案。对于较复杂的任务，花些时间规划通常很有帮助。切换到计划模式，并让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示词：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定适当的实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何内容与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划完成并审查后，就可以开始实现。使用 Autopilot 让 Copilot 按计划迭代解决问题。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所安装版本中含义相同的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。检查代码并运行网站，确认功能正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 根据议题和规划时做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的结果，如有失败，请要求 Copilot 修复。

## 试用新功能

代码看起来没问题，但还需要确认它能运行。像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 测试类别筛选、发行商筛选，以及计划中商定的组合方式。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。在本课中，你：

- 根据 GitHub 议题开始了新的 Copilot 对话。
- 在 Plan 模式中确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，使用[自定义说明][next-lesson]深入了解代码生成，确保生成的代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
