---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按照特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]，指导 Copilot 如何生成代码。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准。
- 再次运行相同提示词，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合部分标准，因为 Copilot 的输出具有不确定性。本练习旨在观察更新说明后行为的*整体变化趋势*，而不是要求生成内容与示例逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应通过 TypeScript 文档注释（TSDoc）为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

通过说明文件，可以向 Copilot 提供这些信息，帮助它遵循上述实践完成任务。

### 自定义说明

自定义说明可提供上下文和偏好，帮助 Copilot 理解你的编码风格和要求。例如，可以指定编码约定、库以及注释规范，从而引导 Copilot 提供更相关的建议和代码片段。说明可以适用于整个存储库，也可以仅适用于特定文件类型或任务。

本项目有两类说明文件：

- `.github/copilot-instructions.md`：每次针对存储库的请求都会发送给 Copilot 的单个说明文件。它应包含项目级信息，即大多数 Copilot 聊天或命令行界面（CLI）请求都适用的上下文，例如技术栈、项目简介、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明文件，例如为 TypeScript、Astro、用户界面（UI）组件或单元测试提供指导。

> [!NOTE]
> 在集成开发环境（IDE）中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot 命令行界面（CLI）和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

创建说明文件的完整过程不在本练习范围内，但示例项目展示了一种常见做法：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保说明清晰，并提供代码示例，说明什么样的代码符合或不符合要求。

说明文件没有唯一的正确写法，AI 的使用方式也不止一种。通过实践可以找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。查看本项目的文件时，你会发现其中有针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前工作环境的相关课程会在需要时介绍具体操作。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

花些时间查看本项目自带的说明文件，其中包括一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网站中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的嵌套 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括针对 Astro、Drizzle 数据层、测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它通过相对于存储库根目录的 glob 模式，限定说明适用的文件。例如，与 `**/*.test.ts` 匹配的 TypeScript 测试文件都适用。
5. 查看本项目中专门针对单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。查看它指向的其他说明文件（如 `unit-tests.instructions.md`）和项目文件。这种方式可以将较大的说明集拆分成可复用的小文件，并引用项目示例供 Copilot 生成代码时参考。文件中的路径相对于该说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

按照最佳实践，先为更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个全新的对话。模式和模型的选择因练习而异；每个相关步骤都会说明具体要求。

为了观察自定义说明的影响，先在当前说明文件不变的情况下发送提示词。稍后更新说明后，再次发送相同提示词并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取无关上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，生成的辅助函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有不确定性，因此即使未明确要求，它也可能添加文档注释。若发生这种情况也没关系，本练习重点是观察更新说明后行为的一致性变化。

## 添加新的存储库标准

前面提到过，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。查看其中记录的项目编码标准；目前没有代码文档规则，因此之前生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件既可以放在存储库级别（`.github/copilot-instructions.md`），为整个项目提供指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。适用于整个项目的标准（如刚添加的文档注释规则）应放在存储库级说明文件中。

## 再次运行提示词并观察变化

现在再次发送相同提示词，观察更新后的说明对生成代码的影响。

1. 返回 Copilot Chat 并选择 **New Chat**，清除当前对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**完全相同的提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看生成的文件是否以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 查看生成的函数是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

现在你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中，下一练习会继续使用这些代码。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，也需要通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并关联待办事项中的文档标准议题。也可以手动完成，但已连接项目的 GitHub 工具，因此让 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用 GitHub 工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍此连接的工作原理——模型上下文协议（MCP）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中既有说明更新，也有为后续筛选工作添加的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果系统提示允许 Copilot 代表你执行 GitHub 操作，请先检查操作内容，再按需选择 **Allow**。
6. PR 创建后，使用以下提示词请 Copilot 合并 PR，并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示允许 Copilot 代表你执行 GitHub 操作和 shell 命令，请先检查命令，再按需选择 **Allow**。

现在你已在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结和后续步骤

在本练习中，你：

- 探索了项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 发送代码生成提示词，观察 Copilot 在使用**现有**说明时生成的内容。
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准。
- 再次运行相同提示词，观察生成的代码如何采用新标准。
- 将说明更新和辅助函数基础提交到 `main`，以便下一练习可以继续使用。

接下来，使用[代理模式][next-lesson]让 Copilot 在整个代码库中添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
