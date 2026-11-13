---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的常见工作流程包括查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次修改……类似步骤还有很多。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的工作方式类似开发者：先探索现有项目结构，再执行所需更新，运行测试等任务，并自动修复发现的问题。现在来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，可快速反馈并随时交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调多文件更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查智能体模式生成的更改和测试。

## 场景

游戏列表不断增加，因此你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数；现在将使用 Copilot 智能体模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台，可列出游戏并显示详情。它是一个单体 Astro 应用，构建时会将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端的 localhost URL 时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，此时 Web 服务器由 Codespace 托管。

### 探索网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的对话。不同练习的模式和模型选择有所不同，相关步骤会在需要时说明。

网站的初始版本可以正常运行，但我们希望增加功能。先查看待办事项。使用模板创建项目时，系统已自动创建一组议题作为待办事项。请 GitHub Copilot 帮你列出这些内容。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此步骤中包含该模型，希望由此获得相对一致的代码建议。但 LLM 的结果具有概率性，你可能会看到与研讨会示例不同的建议。这完全正常。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还根据它认为对用户最有用的程度，为这些事项排了优先级。

## 查看说明文件

让智能体开始生成代码之前，先查看将用于向 Copilot 提供工作上下文的说明文件。你将使用[用户界面 (UI) 说明][ui-instructions]，了解如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和辅助功能原则。
   - 指向各类文件专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置，因此可以复用内容，将复杂说明拆分成更小、更易管理的部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用进行三项独立更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以帮你完成这些任务！现在添加筛选功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图智能体下拉菜单中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。你应该能看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中，代码编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 选项，因此无需在工作过程中选择它们。

7. 按 Copilot 的提示选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认运行测试的截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 可能会暂停，过程中甚至会有测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且不再发现错误。

   ![Copilot 智能体模式的完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 记住，务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、识别相关文件并协调多文件更改。
- 在将代码合并到代码库之前，审查智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 附加探索练习——实现分页

随着游戏列表不断增加，可能需要启用分页。使用本练习中学到的技能，提示 Copilot 为网站添加分页。可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 决定分页的实现方式，例如是否允许用户选择页面大小，还是使用固定大小。
- 编写提示词时，为 Copilot 提供所需指导，确保实现符合预期。
- 你可能需要与 GitHub Copilot 反复协作，要求更改并补充上下文。这是正常的协作流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
