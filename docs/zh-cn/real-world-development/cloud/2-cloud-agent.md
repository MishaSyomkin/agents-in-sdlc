---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而积压的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿拉取请求（PR），供开发人员审查。这样可以将繁琐任务交给智能体处理，加快开发进度，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受聘开发网站的承包商没有留下完善的文档，事实上，几乎完全没有文档。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望每款游戏的详情页都显示相关游戏（即同类别的其他游戏），让访客可以继续浏览。团队目前不要求完善的实现，只需要可用于用户体验（UX）验收测试的版本。这项工作目前受阻，不过团队眼下还有优先级更高的其他议题。

这类工作很容易被搁置，适合分配给 Copilot 云智能体异步处理。开发人员可以先处理其他任务，之后再回来审查 Copilot 的工作，确认结果符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。就像与开发人员协作一样，你可以通过多种方式分配工作，其中包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿 PR 以跟踪进度、设置工作环境并开始处理任务。无论会话仍在进行还是已经完成，你都可以查看其过程。待方案准备好供审查时，Copilot 会在 PR 中通知你。

## 编写范围明确的说明

GitHub Copilot 并非万能工具，使用它仍然需要思考。看似简单的操作也可能包含复杂细节，因此[为 Copilot 云智能体界定任务范围时要谨慎][cloud-agent-best-practices]。可以把它视为人工智能（AI）编程伙伴：分阶段工作、不断学习、尝试并调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成过自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没看过，现在正是快速浏览的好时机。将任务分配给 Copilot 云智能体前，先只读查看存储库中已有的说明文件，以便之后观察其影响。

可在存储库的 GitHub 网站上，或在已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TypeScript 文档注释（TSDoc）要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些说明如何限定适用范围到 `**/*.test.ts` 文件。

下一部分将把标题为 `Code lacks documentation` 的议题分配给云智能体。查看它生成的 PR 时，注意 TSDoc 文档注释、TypeScript 约定和文件注释头；这些都来自上述说明文件。之后的练习还会再次检查这些内容。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行一些设置脚本，确保环境状态良好。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师（SWE）的方式执行任务。

云智能体使用 [GitHub Actions][github-actions]作为工作环境。可通过创建[专用设置工作流][setup-workflow]自定义环境，工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并在云智能体开始工作前运行，以便访问所需的开发工具和依赖项。实验已预先配置此工作流，以简化流程并提供学习机会。该工作流会为 Copilot 安装 Node.js、项目依赖项和端到端测试所需的浏览器，并准备好迁移且已填充数据的单体 Astro 应用 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个要点：

- 它仅包含一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理 PR 前通过 GitHub Actions 执行。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。这有助于验证工作流是否成功，而不必等待 Copilot 调用它。

## 添加文档

大多数项目要么文档过时，要么完全缺少文档，这类技术债务常常得不到处理，降低开发效率，也让代码维护和新成员加入团队变得更加困难。Copilot 擅长创建文档，因此很适合将这项任务分配给云智能体。它会在后台生成所需文档，后续练习会再回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，标题可能带有“工作进行中”（work in progress，WIP）标记，例如 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建工作环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 审查列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让开发人员可以处理其他工作。现在利用这一特性，再分配一个任务给云智能体，同时开始编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

GitHub Copilot 云智能体的一大优势是可以拆分工作：你可以专注于一组任务，同时让它处理其他任务。即使为设计团队添加相关游戏部分不一定要花很长时间，也可以把时间用于其他工作。现在将这项任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的首条评论中会出现一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二个请求。云智能体的工作方式类似软件工程师，因此无需一直监控；待任务完成后再审查即可。现在继续创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步执行任务。Copilot 可用于处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课涉及以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，接下来创建并使用自定义智能体。[Copilot 云智能体也支持模型上下文协议（MCP）服务器][cloud-agent-mcp]，并可使用前面练习中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
