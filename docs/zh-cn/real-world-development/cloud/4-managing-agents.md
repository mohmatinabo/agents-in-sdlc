---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体分别处理了三项任务，目标是改善用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在此 Mission Control 中，可以查看所有有未关闭拉取请求 (PR) 的智能体任务，也可以探索已执行的操作，甚至引导进行中的会话。

本课将：

- 探索智能体页面，监控云智能体任务。
- 引导进行中的会话，请求添加其他功能。

## 场景

将高对比度模式分配给智能体后，团队意识到现在也适合添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此顺便加入此功能合乎逻辑。你将引导智能体，同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作。可能包括：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和采用的方法而有所不同。

4. 同时留意右侧的拉取请求 (PR) 面板。你可以在此查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

了解当前任务后，现在让 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission Control 中的辅助功能会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。出现与以下示例类似的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面中的任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认提示词，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这同样需要几分钟，可以借此暂停一下，回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此 Mission Control 中，你可以跟踪所有进行中和已完成的任务、审查智能体正在执行的工作，甚至重定向进行中的任务以调整范围或提供额外指导。

本课涉及以下概念：

- 使用 Mission Control 和智能体页面监控云智能体任务。
- 重定向进行中的会话，请求添加其他功能。

Copilot 正在完成辅助功能。接下来，[迭代完善 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代完善 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
