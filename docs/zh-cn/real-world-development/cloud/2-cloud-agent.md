---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务。这可能是尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发者专注于更重要的工作。

本练习将使用 Copilot 云智能体探索：

- 如何自定义生成代码的环境。
- 如何确保操作安全。
- 明确限定议题范围的重要性。
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有妥善编写文档——事实上，几乎完全没有文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队已准备好改善游戏发现体验。他们希望在每款游戏的详情页上显示相关游戏（即同类别的其他游戏），让用户可以继续浏览。他们暂时不需要成熟完善的实现，只需要一个可用于用户体验验收测试的版本。虽然这项工作目前受阻，但此时还有优先级更高的议题。

这些任务很容易被推迟，很适合分配给 Copilot 云智能体。它可以异步处理任务，让开发者专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以在后台执行任务，方式与开发者类似。就像与开发者协作一样，你可以通过多种方式启动任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确限定指令范围的重要性

GitHub Copilot 没有魔法，不能替你免去思考。即使看似简单的操作，也可能包含许多复杂细节，因此[为 Copilot 云智能体明确限定任务范围][cloud-agent-best-practices]非常重要。把它当作 AI 结对程序员：分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已完成自定义说明的实践练习，就已经见过 `.github/copilot-instructions.md` 的作用；如果尚未学习，现在正好快速查看一下。将工作分配给 Copilot 云智能体前，先只读查看存储库中已有的说明文件，以便稍后观察它们的影响。

在 GitHub 网页界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释方面的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些说明限定为适用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述说明文件。之后的练习审查 PR 时还会再次提到这些要求。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境就绪。将任务分配给以类似软件工程师方式工作的 Copilot 时也是如此。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行，以便访问所需的开发工具和依赖项。实验已预先配置此工作流，以便顺利完成练习。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用所需的已迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为步骤设置所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 例如，如果设置步骤需要克隆存储库以安装依赖项，则需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流文件类似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，以便先确认工作流运行成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但大多数项目中的文档要么过时，要么缺失。这类技术债务经常无人处理，会拖慢工作效率，也让维护代码库或帮助新开发者加入团队变得更困难。Copilot 擅长创建文档，因此这是分配给 Copilot 云智能体的理想任务。它会在后台生成所需文档，之后的练习中你会回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚生成的拉取请求 (PR)，其标题类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳解决方案。

13. 查看 Copilot 将要完成的任务列表。
14. 向下滚动拉取请求时间线，你会看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 留意你可以查看实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并决定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要优势之一就是异步执行任务，让我们可以专注于其他工作。接下来，我们会利用这一点：先将另一个任务分配给云智能体，再转而为应用编写代码以添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏部分不一定需要很长时间，这段时间也可以用于其他工作。现在将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在议题的首条评论中看到 👀，表示 Copilot 已开始处理任务！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。接下来开始创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将议题分配给 Copilot，让它异步处理任务。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课涉及以下概念：

- 自定义生成代码的环境。
- 确保操作安全。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可使用之前模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
