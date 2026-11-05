---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档规范，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按照特定方式完成，应让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述想要编写什么代码，还描述代码应如何组织。构建完筛选功能后，你将探索 Copilot 使用的说明、添加文档规范，并将其应用到代码中。

在本课中，你将：

- 了解存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TypeScript 文档注释（TSDoc）或 JavaScript 文档注释（JSDoc）说明其用途、参数和返回值；如有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时应同步更新注释。
- 应保留现有格式和 lint 指南。

使用说明文件可向 Copilot 提供这些信息，使其按团队实践完成任务。

## 说明文件

自定义说明可提供上下文和偏好，帮助 Copilot 理解编码风格和要求。例如，可以指定编码约定、库以及注释规范。说明可以适用于整个存储库，也可以仅适用于特定文件类型。

本项目有两类说明文件：

- `.github/copilot-instructions.md`：每次针对存储库的请求都会发送给 Copilot 的说明文件，应包含大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况因工作环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

开始修改前，先查看入门项目中已有的说明文件，以便之后观察更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 阅读项目简介和编码指导。此文件适用于 Copilot 在此存储库中的所有交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件，包括针对 Astro、Drizzle 数据层、测试等的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它通过 glob 模式限定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他说明文件和项目文件。这种方式可以将较大的说明集拆分成可复用文件，并引用可供 Copilot 参考的示例。

## 更新说明文件以符合团队指导

现有文件已打下良好基础，但还需补充 TypeScript 文档注释要求。修改核心 `copilot-instructions.md` 文件：

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 在该部分的最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存说明后，退出并恢复筛选对话，使 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录运行以下命令：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 请 Copilot 按照更新后的指导修改代码：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改过的 TypeScript 文件。检查新生成的 TSDoc 注释是否准确说明代码。

## 总结和后续步骤

你探索了 Copilot CLI 如何从说明文件获取上下文，并将新的规范应用到功能中。在本课中，你：

- 探索了存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
