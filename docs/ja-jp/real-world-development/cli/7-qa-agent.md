---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` skill を使って自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。今度は、これらの機能を明確な QA プロセスを持つ custom agent にまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool と連携する仕組みを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA agent を選択し、フィルタリング issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト（PR）を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫して確認する必要があります。custom agent を使うと QA プロセスを調整し、再利用できるレポートを作成できます。

## Custom agent とは

Custom agent は、Markdown プロファイルで定義する Copilot の特化版です。プロファイルには、エージェントの目的、instruction、利用可能な tool を記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を記述します。`quality-checks` skill は繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらをどう使って要件を評価し、結果を報告するかを Copilot に指示します。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用できる QA プロファイルを Copilot に作成してもらいます。プロファイルには QA が行うチェックと、守るべき範囲を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive mode であることを確認します。
2. 既定のエージェントに、次の custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しい agent を使う前に、Copilot が意図した QA ワークフローと制約を正しくプロファイルに記載したことを確認します。これにより、検証だけを依頼したのに、プロファイルが不完全だったり範囲を超えた変更を行ったりすることを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略できますが、含めると agent の表示名が明確になります。
3. プロファイルの instruction を読み、QA が要件を確認し、リポジトリの instruction に従い、`quality-checks` skill と Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更を commit したりプルリクエストを開いたりしないことを確認します。
5. これらの責務や制約が抜けている場合は、続ける前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開し、QA を選択します。そうすると、issue と計画の内容をコンテキストとして使えます。

1. 次のプロンプトで agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> agent を作成した直後は、利用可能な agent の一覧に表示されない場合があります。agent は作成済みなので、上記のコマンドで有効にできます。

2. 次のプロンプトを使い、QA agent に機能をレビューしてもらいます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を始めます。
4. 完了したら、結果のレポートを読みます。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに加え、その結果を確認しました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP tool と連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA agent を選択し、フィルタリング issue に照らして結果をレビューしました。

実装、skill の更新、QA プロファイル、テスト、検証レポートが揃いました。次は、これらを [機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [Custom agent の設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
