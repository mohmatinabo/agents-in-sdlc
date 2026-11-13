---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解你想让它做什么，也了解你希望它如何完成。

本练习将：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会使用“更新前/更新后”的提示词进行对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。之后审查云智能体生成的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

使用说明文件可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。这是引导它提供更相关建议和代码片段的有效方式。你可以指定偏好的编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建。你可以为 TypeScript 或 Astro 等特定语言，或创建 UI 组件、新建一组单元测试等任务提供指南。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

全面讨论如何创建说明文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 用于项目级指南，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门说明。
- 使用自然语言，保持指南清晰，并提供代码应如何编写（以及不应如何编写）的示例。

创建说明文件没有唯一的方法，正如使用 AI 也没有唯一的方法。通过不断尝试，你会找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一组完善的说明文件。查看本项目中的说明时，你可能会发现有些文件针对多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前工作环境的相关课程会在适用时介绍具体操作。
>
> 想寻找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义说明文件

花些时间阅读此存储库自带的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看文件中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些说明。
5. 留意此项目中有关如何创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。查看指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 提供可参考的示例。（其中的路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求在代码中添加文档注释。接下来的步骤将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要进行本练习唯一的编辑：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释。之后，你分配给 Copilot 的议题将使用这条说明。

编辑前，先创建一个分支（云智能体会从议题目标分支读取说明，但仍建议干净地提交这些编辑）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录存储库编码标准，以便改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。留意其中记录了项目编码标准，但还没有关于代码内文档的规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件 (`.github/copilot-instructions.md`) 中创建全局说明，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。刚才添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并完成合并。下一练习分配议题时，云智能体便会读取这些说明。

> [!TIP]
> 如果希望本次研讨会直接在 `main` 上操作，可以跳过创建分支，直接提交到 `main`。保留分支步骤是为了让研讨会更贴近真实项目工作流。

## 总结与后续步骤

你已了解 Copilot 如何从项目说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。后续练习中，你会看到云智能体生成的拉取请求如何遵循该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
