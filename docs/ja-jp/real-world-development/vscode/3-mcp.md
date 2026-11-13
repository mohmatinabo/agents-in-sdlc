---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agent →][next-lesson]

Agent mode を使ってフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol（MCP）と、MCP server が Copilot に tool を追加する仕組みを学びます。
- workspace に Playwright MCP server を追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外にアクセスできると、さらに強力になります。Model Context Protocol (MCP) は Copilot が外部に接続する方法です。エージェントが外部の tool やサービスと通信するための標準的なプロトコルです。

![コンテキスト、LLM、tool（MCP server と VS Code 拡張機能が提供する tool を含む）と agent mode の連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI agent が外部の tool やサービスと通信する仕組みを提供します。MCP を使うと、AI agent は外部の tool やサービスとリアルタイムに通信し、最新情報（resource）にアクセスしたり、操作（tool）を代わりに実行したりできます。

これらの tool や resource には、AI agent と外部の tool やサービスをつなぐ MCP server を通じてアクセスします。MCP server は、AI agent と既存の API や NPM package などのローカル tool との通信を管理します。それぞれの MCP server は、AI agent が利用できる tool と resource のセットを提供します。

よく使われている MCP server には、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。AI agent は新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI agent は Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまな tool や resource を利用できる MCP server がほかにも公開されています。GitHub は、エコシステム内での発見やコントリビューションを促すために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点では、MCP server をプロジェクト内のほかの依存関係と同じように扱ってください。利用する前にソースコードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP server のみを使い、機密情報や重要な操作へのアクセスを許可する際は慎重に判断してください。

## MCP の設定を確認する

`.vscode/mcp.json` file で、この VS Code workspace で利用できる MCP server を設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` server が設定されていることを確認します。

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

この `github` の設定はプロジェクトテンプレートに含まれているため、前の演習では Copilot が issue のバックログを読み取れました。これは[リモート GitHub MCP server][remote-github-mcp-server] を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めて tool を使うときに GitHub で認証します。

## Playwright MCP server を追加する

次に、2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は Copilot が操作できるブラウザーを提供するため、機能のテストに適しています。

1. `.vscode/mcp.json` で `github` の隣に `playwright` entry を追加し、file の内容を次のようにします。

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

2. file を保存します。

リモートの `github` server とは異なり、`playwright` は**ローカル**server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` flag を指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトは end-to-end test に Playwright を使っているため、必要なブラウザーはすでにインストールされている場合がほとんどです。Copilot からブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

## Server を起動して信頼する

VS Code は MCP server を**必要になったときに**起動します。Copilot が server の tool を初めて使う際、VS Code が server を起動して、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` entry の上に **Start** 操作が表示されます。選択して server を起動し、設定が正しいことを確認します。
2. server を信頼するか確認された場合は、設定を確認し、信頼して起動できるようにします。

手動での起動は省略できます。その場合、次の手順で Copilot が tool を初めて使うときに VS Code が server を起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントがページを開き、移動し、filter を適用して、その結果を読み取って要約します。会話を離れずに機能が期待どおりに動作することを確かめる、手早い方法です。

内部的には、Playwright MCP server はスクリーンショットではなく、ページの [accessibility tree][playwright-mcp-server] を使います。つまり、エージェントは支援技術と同じように、ボタン、リンク、リスト項目などの構造化されラベル付けされた要素を判断材料にします。機能の簡単な確認が、アクセシビリティの基本的なチェックにもなります。

server に接続し、アプリが起動したら、作成したフィルタリング機能を Copilot に確認してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server を通じてブラウザーを起動し、各手順を実行して確認結果を報告します。issue の受け入れ条件に照らして要約を読み、問題があれば追加質問をしたり、プルリクエストを開く前にコードの修正を依頼したりします。

> [!NOTE]
> このテストを行うには、アプリを `http://localhost:4321` で起動しておく必要があります。dev server を停止している場合は、prompt を送る前にもう一度起動してください。Copilot が Playwright MCP server を初めて使うときは、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が正しく動作することを確認できました。次は、チームがレビューできるようにプルリクエスト（PR）を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit message を生成します。

   ![変更内容を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub command-line interface (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連 issue を見つけ、issue と関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. agent dropdown で **Agent** が選択されていることを確認し、Copilot が GitHub tool を使えるようにします。
3. 次の prompt で PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の取得や操作を実行するために Copilot が **Continue** を求めたら、必要に応じて選択します。GitHub tool を初めて使うときに GitHub での認証を求められる場合があります。画面の指示に従って認証してください。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol（MCP）と、MCP server が Copilot に tool を追加する仕組みを学びました。
- workspace に Playwright MCP server を追加しました。
- ブラウザーでフィルタリング機能を手動テストするために利用しました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP server を使って機能を*テスト*しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティのレビューなど、対象を絞ったタスクを効率化する [custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
