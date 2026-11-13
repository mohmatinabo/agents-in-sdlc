---
title: "练习 3 - 自定义智能体"
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

你将通过自定义智能体了解：

- 如何定义自定义智能体。
- 如何向自定义智能体分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前深色主题难以阅读，因为文本与背景颜色的对比度不足。为解决这一辅助功能问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

辅助功能至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成相关功能。

## 检查辅助功能自定义智能体

我们已为你创建了辅助功能自定义智能体。先查看内容，了解它将如何引导 Copilot。

返回 Codespace，然后检查辅助功能自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 继续阅读后续部分，查看其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 辅助功能最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配议题

Mission Control 是集中管理环境中所有智能体的位置。你可以在此将任务分配给 Copilot 云智能体、监控任务，甚至重定向并提供额外指导。现在将实现高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![Copilot 云智能体分配界面，突出显示自定义智能体和辅助功能智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务！

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。通过自定义智能体，团队可以将专业知识和标准整理成可复用智能体，从而引导 Copilot 更有效地完成特定类型的工作。

本课涉及以下概念：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，前往 Mission Control [监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
