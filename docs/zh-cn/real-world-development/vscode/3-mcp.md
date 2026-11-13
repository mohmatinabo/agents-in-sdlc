---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求之前，应该在浏览器中确认功能确实正常。无需自己逐一操作应用，你将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，然后发布分支并创建 PR。

本练习将：

- 了解什么是 Model Context Protocol (MCP)，以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当智能体模式可以访问编辑器以外的工具时，能力会大幅增强。Copilot 通过 Model Context Protocol (MCP) 实现这一点：它是智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理示意图，展示它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务（例如现有 API 或 NPM 包等本地工具）之间的桥梁，并负责管理双方之间的通信。每个 MCP 服务器都代表一组智能体可以访问的工具和资源。

一些常用的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建新存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体可以打开网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以访问不同工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 与项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 条目由项目模板提供，因此 Copilot 在上一练习中可以读取待办事项中的议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容——VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时让你使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正适合用于测试功能。

1. 在 `.vscode/mcp.json` 中的 `github` 条目旁添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

远程 `github` 服务器与本地 `playwright` 服务器不同：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动 Playwright。`--headless` 参数表示 Playwright 在后台运行浏览器，不显示窗口。Codespace 中没有桌面环境，因此必须使用此参数。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动它，并询问你是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。无需亲自点击应用检查结果，智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果。这是在不离开对话的情况下快速确认功能是否符合预期的方式。

Playwright MCP 服务器在底层使用页面的[辅助功能树][playwright-mcp-server]，而不是截图。因此，智能体会根据带标签的结构化元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式相同。这意味着快速的功能检查也可以顺便验证基本辅助功能。

连接服务器并启动应用后，让 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成测试，并报告结果。根据议题中的验收标准审查报告；如果发现问题，可以继续提问或让 Copilot 修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示词前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查。首先发布 `filtering-vscode` 分支。

1. 打开 Codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，包括使用 github.com 和 GitHub 命令行接口 (CLI)。既然你已在使用 GitHub Copilot，不妨让它来创建 PR！它可以找到相关议题并将 PR 与议题关联起来。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉菜单中选择 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，系统可能会提示你登录 GitHub，按照提示授权即可。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接以查看拉取请求，但**暂时不要合并**。

## 总结与后续步骤

恭喜！本练习中，你：

- 了解了什么是 Model Context Protocol (MCP)，以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在浏览器中手动测试了筛选功能，然后再发布。
- 发布了分支，并为筛选工作创建了拉取请求。

你已经使用 MCP 服务器*测试*功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，以便简化辅助功能审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
