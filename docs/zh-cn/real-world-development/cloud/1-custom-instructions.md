---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按照特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]可提供相关指导，让 Copilot 不仅理解要做什么，也了解应如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会比较提示词更改前后的效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示差异。之后审查云智能体创建的拉取请求时，可以看到说明文件的影响。

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

## 添加新的存储库标准

本练习中唯一需要编辑的内容，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。之后将议题分配给 Copilot 云智能体时，它会读取这条规则。

编辑前，先创建分支（云智能体会读取议题目标分支中的说明，不过仍应通过提交妥善管理更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习将把议题分配给 Copilot，届时 Copilot 会从 `main` 分支创建新分支。因此，必须先将说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。下一练习中，云智能体处理分配给它的议题时便会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上操作，可以跳过创建分支的步骤，并直接提交到 `main`。此处创建分支是为了让研讨会流程贴近真实项目的工作方式。

## 总结和后续步骤

在本练习中，你：

- 探索了项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加了一条适用于整个存储库的新标准。

之后审查云智能体生成的拉取请求时，可以看到这条标准的实际应用。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
