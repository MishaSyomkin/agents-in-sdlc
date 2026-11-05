---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多项改善用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及添加高对比度和浅色模式切换等无障碍功能。现在查看代码更改，并在需要时向 Copilot 提供反馈，改进其工作。

### 场景

前面多次提到，引入生成式人工智能（AI）并不会改变软件设计和开发运维（DevOps）的基本原则。始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队其他成员启用审查之前，先查看 GitHub Copilot 对文档、相关游戏功能和无障碍功能的实现。

## 安全性和 GitHub Copilot 云智能体

由于 Copilot 云智能体异步且无人监督地执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，对代码只有写入其工作分支的权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境来执行工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对模型上下文协议（MCP）服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot 云智能体生成的第一个拉取请求（PR），其中包含为代码添加的文档。本节使用 GitHub.com 的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期情况**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR，具体标题可能有所不同。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含“工作进行中”（work in progress，WIP）标记 **[WIP]**。此时请等待 Copilot 完成工作，这可能需要几分钟。等待时可以休息片刻，或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TypeScript 文档注释（TSDoc）和其他文档。具体更改可能有所不同。

   查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些来自练习 2 中查看的自定义说明文件。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应在 PR 的检查部分排队运行。正常情况下，单体 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新会话处理修改。由于生成结果并非确定，因此无法提供固定的请求文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，查看 Copilot 如何处理修改。注意它会启动新会话来进行更新。
3. 选择 **Back to pull request** 可返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。确认满意后，可将 PR 从草稿状态改为准备审查，并合并到主分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，具体标题可能有所不同。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的指示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应在 PR 的检查部分排队运行。正常情况下，单体 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。
9. **可选：** 可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能。与之前一样，可以继续迭代并要求 Copilot 修改，例如调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（注意，这个议题是在更新自定义说明**之前**分配的）。与之前一样，可在 **Conversation** 选项卡中添加评论，让 Copilot 启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，具体标题可能有所不同。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含“工作进行中”（work in progress，WIP）标记 **[WIP]**。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，重点检查：
   - 用于切换模式的用户界面（UI）组件。
   - 使用本地存储持久保存用户偏好的方式。
   - 高对比度和浅色模式的层叠样式表（CSS）或其他样式更改。
   - 无障碍富互联网应用（ARIA）属性、键盘导航等。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应在 PR 的检查部分排队运行。正常情况下，单体 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认功能符合预期，且页面重新加载后设置仍然保留。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求修改。

## 可选练习 — 继续委派任务

云智能体最适合处理真实待办事项，让你能同时专注于其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，以培养这种协作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境的练习。在这些课程中，你：

- **检查了存储库自带的自定义说明文件，**以便之后观察云智能体输出中的影响。
- **将议题分配给 Copilot 云智能体，**并观察它设置环境、制定计划和异步执行任务。
- **创建并使用了自定义智能体，**为无障碍功能添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为 mission control，**监控并引导正在运行的无障碍会话。
- **审查并迭代了云智能体的拉取请求，**通过标记 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

云智能体工作环境的练习已经完成。如果希望继续探索，其他工作环境可补充不同的实践内容：

- **[VS Code 工作环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- **[命令行界面（CLI）工作环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体连接方式。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
