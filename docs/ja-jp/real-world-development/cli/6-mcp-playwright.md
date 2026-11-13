---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを使ったり、Copilot が利用できる自動化を増やしたりする必要があります。MCP server を使うと、Copilot CLI に組み込まれている機能に加えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol（MCP）と Copilot CLI での使われ方を理解します。
- まだ利用できない場合は、Playwright MCP server を追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認します。

## シナリオ

ユニットテストや end-to-end test は重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot に利用者と同じようにサイトを使わせることで、変更が期待どおりに動作する確信を高め、作業をさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI agent は外部のツールやサービスとリアルタイムに通信し、最新情報へのアクセスや操作の実行ができます。

これらのツールやリソースには、AI agent と外部のツールやサービスをつなぐ MCP server を通じてアクセスします。それぞれの MCP server は、AI agent が利用できるツールやリソースのセットを提供します。

よく使われている MCP server には、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP server が公開されています。GitHub は、エコシステム内での発見やコントリビューションを促すために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP server は、プロジェクト内のほかの依存関係と同じように扱ってください。利用する前に、ソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot が利用者と同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP をすでに設定している場合は、セットアップ手順をスキップして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP server の追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server type が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP の画面を閉じます。

## Playwright を使って Copilot に機能を確認させる

前の手順では、機能が期待どおりに動作することを手動で確認しました。今度は、Copilot が利用できる Playwright MCP server を使って同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCP server で機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は、Playwright MCP server を使うよう Copilot に明示しなくても、自動で判断します。ただし、使用するツールが分かっている場合は、プロンプトに含めると結果の一貫性を高め、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を確認します。
3. 結果の報告を読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Playwright MCP server を使い、Copilot CLI から実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol（MCP）と Copilot CLI での使われ方を学びました。
- Playwright MCP server がまだ利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、skill とブラウザーツールを組み合わせる [QA custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
