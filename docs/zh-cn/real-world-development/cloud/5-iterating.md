---
title: "练习 5：迭代完善 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多项任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队构建可迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的辅助功能。现在来检查代码更改，必要时向 Copilot 提供反馈，改进它的工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。基于这一点，现在审查 GitHub Copilot 为代码文档、相关游戏功能和辅助功能所做的更改，然后再为团队其他成员开启审查。

## 安全与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此我们设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 只能读取存储库，并且**只能**写入用于保存其代码的分支。
- 云智能体在 GitHub Actions 中运行，会创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体生成的第一个拉取请求 (PR)，其中包含为代码添加的文档。你将使用 GitHub.com 中的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。如有需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更完整描述的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下，也可以回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到部分工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流进入拉取请求检查列表的队列。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 合作并不是单向的。你也可以像提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供固定的请求文本。你可以要求 Copilot：

- 在每个代码文件开头添加简短的文件说明注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 在评论中使用 **@copilot** 提及 Copilot，并要求它修改生成的文档。可以使用上面的任一建议，或提出其他代码文档改进意见。
2. 选择 **View Session**，查看 Copilot 如何执行任务。留意 Copilot 会启动新会话来应用更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。之后，可以将 PR 从草稿改为准备审查状态，再合并到 main 分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更完整描述的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到部分工作流正在等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流进入拉取请求检查列表的队列。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，你可以通过迭代来请求云智能体更新。例如，要求 Copilot 调整显示的相关游戏数量，或确保添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时你还没有更新自定义说明！）。和之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查辅助功能

最后，审查使用辅助功能自定义智能体实现的功能。此 PR 应同时包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的 Mission Control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更完整描述的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 是否使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 辅助功能属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到部分工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流进入拉取请求检查列表的队列。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到该分支，手动测试辅助功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常且在重新加载页面后仍然保留设置。

留意辅助功能自定义智能体如何引导 Copilot 按照辅助功能最佳实践实现这些功能。如果发现辅助功能问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云智能体适合处理真实待办事项，让你能专注于其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，逐步养成这种习惯。以下是一些建议：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境的学习。在这些课程中，你：

- **检查了存储库自带的自定义说明文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并观察它异步设置环境、制定计划和执行任务。
- **创建并使用了辅助功能自定义智能体**，添加高对比度和浅色模式切换开关。
- **使用 Copilot agents 页面作为 Mission Control**，监控并引导进行中的辅助功能会话。
- **审查并迭代了云智能体创建的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾与后续步骤

你已完成云智能体工作环境的学习。若想继续探索，其他工作环境可以补充本次实践：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，可以尝试以下后续任务：

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
