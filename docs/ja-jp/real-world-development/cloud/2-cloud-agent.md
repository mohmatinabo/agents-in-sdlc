---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instruction][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな課題があります。GitHub Copilot cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできる draft PR を作成します。これにより、手間のかかるタスクを委任し、開発を加速させ、開発者がより大きな課題に集中できるようになります。

この演習では、Copilot cloud agent を使って次のことを確認します。

- コード生成環境のカスタマイズ
- 操作を安全に実行する方法
- 範囲を明確に定めた issue の重要性
- Copilot への issue の割り当て

## シナリオ

Tailspin Toys には解消したい技術的負債があります。サイトの最初のバージョンを作った開発者がドキュメントを残していません。まず、アプリケーション内で export されているすべての関数に TSDoc の doc comment を追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーの別のゲームを「関連ゲーム」として表示し、利用者がほかのゲームも見られるようにします。まだ仕上げる必要はなく、UX の受け入れテストに使える状態にしたいと考えています。これは現在のブロッカーですが、さらに優先度の高い issue もあります。

どちらも、優先度を下げられがちなタスクの例です。Copilot cloud agent に割り当てれば、非同期に作業を進めてもらえます。その間、開発者は別のタスクに取り組み、後から Copilot の成果をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると Copilot は進捗を記録する draft pull request を作成し、環境を準備してタスクに取りかかります。作業中も完了後もセッションの内容を確認できます。提案された解決方法のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法のような力があるわけではありません。考える作業を省略することはできません。一見簡単そうな作業でも、詳しく見ると複雑な点があります。[Copilot cloud agent に依頼するタスクの範囲設定に注意してください][cloud-agent-best-practices]。AI ペアプログラマーとして、段階的に作業し、学び、試し、必要に応じて調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

前の演習では、custom instruction と Copilot へのガイダンスの伝え方を確認しました。custom instruction の演習をすでに完了していれば `.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、この機会に簡単に見ておきましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された instruction file を読み、後でその影響を確認できるようにします。

リポジトリを github.com で開くか、Codespace が起動中であれば Codespace で次の file を開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この instruction は `**/*.test.ts` file に適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、作成されたプルリクエストに TSDoc の doc comment、TypeScript の規約、コメントヘッダーが反映されていることを確認できます。後の演習で PR をレビューするときに、改めて説明します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成する際は、誰が作業する場合でも、適切な環境とセットアップスクリプトが必要です。タスクを Copilot に割り当てる場合も同様で、Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は GitHub Actions を使って作業環境を用意します。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行する環境をカスタマイズできます。これにより、必要な開発 tool や依存関係を利用できるようになります。このラボの流れを円滑に進められるよう、事前に設定済みです。Cloud agent が Node.js、プロジェクトの依存関係、end-to-end test 用ブラウザー、単一 Astro app 用にマイグレーションと seed を済ませた SQLite database を利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub workflow file と同じですが、重要な点が 2 つあります。

- `copilot-setup-steps` という単一の job が含まれています。この job は Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger が設定されています。リポジトリの **Actions** タブから手動で workflow を実行できるため、Copilot に実行してもらう前に正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、ドキュメントがまったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメント作成が得意なので、Copilot cloud agent に依頼するのに適した issue です。バックグラウンドでドキュメントを作成してもらい、後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo list を作成したことを確認します。

> [!NOTE]
> Copilot が PR に todo list を表示するまで、数分かかることがあります。Copilot は環境を作成し（先ほど確認した workflow を実行します）、プロジェクトを分析して、最適な方法を検討しています。

13. todo list と、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーを更新する必要がある場合があります。

16. セッションをスクロールして、Copilot が作業を進める様子を確認します。コードを調べて状態を理解し、適切な計画を考え、コードを作成する過程が表示されます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行し、利用者が別の作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを Copilot cloud agent に割り当ててから、アプリケーションの機能を追加する作業に移ります。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザインチーム向けに関連ゲームの表示を追加する作業に時間がかからないとしても、その時間をほかのタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![目の絵文字で issue の作業中を示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアのように作業するため、完了まで常に監視する必要はありません。後で結果をレビューします。次は custom agent の作成と使用に進みましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を確認しました。Cloud agent に issue を割り当てると、Copilot が非同期に作業できます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに活用できます。

次のことを学びました。

- コード生成環境のカスタマイズ
- 操作を安全に実行する方法
- 範囲を明確に定めた issue の重要性
- Copilot への issue の割り当て

Cloud agent がバックグラウンドで作業している間に、custom agent の作成と使用に進みます。[Copilot cloud agent は MCP server も使用できます][cloud-agent-mcp]。また、前のモジュールで扱った custom instruction も利用します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow に関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
