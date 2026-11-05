---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员的工作流程往往包括查找所有相关文件、进行修改、运行测试、调试、发现遗漏的文件，再次修改……还可能有更多步骤。

Copilot 代理模式正是为此而设计的。

Copilot 代理模式可以在集成开发环境（IDE）中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再进行必要修改、运行测试等任务，并自动修复发现的问题。接下来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但适用于两种不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot 代理模式如何探索项目、确定相关文件并协调多处更改。
- GitHub Copilot 代理模式如何跨用户界面和数据层实现新功能。
- 合并到代码库前，如何审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏列表不断增长，因此希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成其余数据层、用户界面和测试工作。

## 运行 Tailspin Toys 网站

更改网站之前，先查看 Tailspin Toys 网站，了解它目前的功能。

此网站是一个以开发者为主题的桌游众筹平台，可用于列出游戏并显示其详情。它是一个单体 Astro 应用，构建时会将页面渲染为静态超文本标记语言（HTML）。页面通过 Drizzle 对象关系映射（ORM）在 frontmatter 中直接查询本地 SQLite 数据库；没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则使用少量作用域限定的 Astro `<script>` 和文档对象模型（DOM）API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示包含 URL 的横幅，内容类似如下：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中从终端选择 localhost 地址链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 Codespace 的私有隧道，此时 Web 服务器已在 Codespace 中运行。

### 探索网站

网站启动后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个全新的对话。模式和模型的选择因练习而异；每个相关步骤都会说明具体要求。

网站的初始实现已具备基本功能，但还可以添加新能力。先查看待办事项。使用模板设置项目时，系统已自动创建议题列表；现在请 GitHub Copilot 显示这些事项。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未表示偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，以提供良好体验。不过，大型语言模型（LLM）的输出具有不确定性，收到的建议可能与研讨会示例不同。这完全正常，符合预期。

> [!NOTE]
> 由于 LLM 输出具有不确定性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 发送以下提示词，请 Copilot 查看议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还根据对用户的帮助程度为事项排定了优先级。

## 查看说明文件

让智能体生成代码前，先查看用于向 Copilot 提供工作上下文的说明文件。本练习将使用 [user interface (UI)][ui-instructions] 文件，其中包含为网站添加功能时应遵循的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能时应遵循的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向特定文件类型说明的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于当前说明文件的位置。这样既能复用内容、将复杂说明拆分成较小文件，也能提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用进行以下三项更新：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选界面。

此外，合并前需要运行并通过测试。Copilot 代理模式可以执行这些任务，现在来添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图智能体下拉列表中仍选中 **Agent**。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择模型 **Claude Sonnet 4.5**。
5. 使用以下提示词，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。它应会找到数据层辅助函数、用户界面和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作进行时选择。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板中提示用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否有潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了：

- Copilot 代理模式如何探索项目、确定相关文件并协调多处更改。
- GitHub Copilot 代理模式如何跨用户界面和数据层实现新功能。
- 合并到代码库前，如何审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习——实现分页

随着游戏列表不断增长，网站将需要分页功能。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。编写提示词时可考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定值。
- 编写提示词时，提供足够指导，让 Copilot 按预期实现功能。
- 可能需要与 GitHub Copilot 反复协作，提出修改并提供上下文。这是使用 Copilot 的常见流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
