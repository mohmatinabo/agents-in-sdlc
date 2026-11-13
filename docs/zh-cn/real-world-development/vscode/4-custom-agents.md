---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可以为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如确保符合 [WCAG][wcag] 要求的辅助功能智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局目录 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter（至少有 `name` 和 `description`），后面跟着定义智能体行为、专业知识和指令的 Markdown 提示词。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]之间有一定重叠。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行任务。最简单的区分方式是：**自定义智能体**是执行任务的工作者，**技能**是工具。

自定义智能体有自己的上下文窗口，可在工作时协调技能（甚至其他智能体）。在本实验中，辅助功能自定义智能体会根据辅助功能指南审查并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能来完成工作。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方式。与 AI 的其他用法一样，需要在适合自己环境和场景的方式下测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前深色主题难以阅读，因为文本与背景颜色的对比度不足。为解决这一辅助功能问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

辅助功能至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成相关功能。

本练习将：

- 查看现有的辅助功能自定义智能体。
- 在 Copilot Chat 中使用辅助功能智能体实现高对比度模式。

## 检查辅助功能自定义智能体

我们已为你创建了辅助功能自定义智能体。先查看内容，了解它将如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 创建用于辅助功能工作的新分支（练习 3 的筛选 PR 将保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 继续阅读后续部分，查看其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 辅助功能最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 目录定义的所有自定义智能体。选择自定义智能体后，该聊天会话将遵循该智能体的指令并使用其工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的对话。不同练习的模式和模型选择有所不同，相关步骤会在需要时说明。

1. 如果尚未选择，请从 Chat 视图的智能体下拉菜单中选择 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（它会显示当前智能体，默认是 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向辅助功能智能体发送以下提示词：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 将开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和可审查的差异。
6. 随着编辑应用，**Files changed** 指示器会更新，显示智能体已修改的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 正在实际修改存储库，可能会更新 Astro 组件、CSS 和相关测试等现有文件。

下一课将审查并引导这项进行中的工作。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。通过自定义智能体，团队可以将专业知识和标准整理成可复用智能体，从而引导 Copilot 更有效地完成特定类型的工作。

本课涉及以下概念：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式下使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，在更改进行时审查它们，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
