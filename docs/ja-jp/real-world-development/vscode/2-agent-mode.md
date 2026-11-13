---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数の file を変更し、test などを実行する必要があります。開発者は必要な file を探して変更し、test を実行し、問題をデバッグし、変更し忘れた file を見つけ、さらに修正するという作業を繰り返します。

このようなときに Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業するよう設計されています。まず既存のプロジェクト構造を調べ、必要な更新を行い、test などを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。Agent mode は IDE 内で作業し、素早くフィードバックを得ながら対話できます。Cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連 file を特定し、複数の変更をまとめて行う方法
- GitHub Copilot Agent Mode が UI と data layer にわたる新機能を実装する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更と test をレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリーと販売元で絞り込めるようにします。前の演習では publishers helper を追加しました。ここでは、残りの data layer、UI、test の作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro app で、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite database に直接問い合わせます。別の backend API や client-side UI framework はありません。再利用可能な data-access helper は `src/lib/` に置き、必要な対話機能は標準 DOM API を使う、スコープを限定した小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に起動できるように、単一 Astro dev server を起動する `npm run dev` task が用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば、`main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押して、Codespace 内に新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server の準備ができると、次のような URL を含む案内が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro server のアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択してください。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace で Web server をホストするためのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **Home Page**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **Game Details Page**: ゲームを選択すると、そのタイトル、説明、販売元、カテゴリーなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。mode と model の選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトを作成したときに issue のバックログも自動で作成されています。GitHub Copilot に内容を表示してもらいましょう。

1. Chat ビューの agent dropdown で **Agent** を選択します。**Agent** は自律的に計画し、複数 file にわたる変更を実装し、ターミナルコマンドを実行して tool を呼び出します。

   ![Chat ビューの agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能な model の一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定の model を推奨しているわけではありません。ワークショップを作成する際に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案の一貫性を高めてよい体験を提供することが目的ですが、LLM の動作は確率的なので、ワークショップの例とは異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の動作は確率的であるため、Copilot が別の MCP command を使う場合がありますが、タスク自体は完了できるはずです。

3. 次の prompt を Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示する command を実行します。
5. 生成された issue の一覧を確認します。

Copilot が、利用者に最も役立つと考えた項目を優先順位付けしていることを確認してください。

## Instruction file を確認する

コードの生成を agent に依頼する前に、Copilot にコンテキストを伝える instruction file を確認しましょう。Web サイトに機能を追加する方法が記載された [user interface (UI) instruction file](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。たとえば、次の内容が含まれています。
   - アーキテクチャの概要
   - component 設計、テスト可能性、アクセシビリティの原則
   - 次のような file 種別ごとの instruction file へのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Instruction file から、ほかの instruction file やプロジェクト内の file を参照できます。パスは instruction file の場所からの相対パスです。これにより、ガイダンスを再利用しやすく小さな単位に分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも 3 種類の更新が必要です。

- data layer（`src/lib/`）の残りのフィルタリングロジックを追加または改善する
- フィルタリングの動作を確認する test を追加または更新する
- ゲーム一覧ページにフィルタリング UI を追加する

さらに、コードベースにマージする前に test を実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、instruction file を追加します。

   ![UI instruction file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)

4. model が引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次の prompt で Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連する file を探す様子を確認します。data layer の helper、UI、test が見つかり、file の変更と test の実行が始まります。

   ![Copilot がプロジェクト内の file を調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、file の変更、test の実行などを行うため、タスクの複雑さやコードベースによって数分かかることがあります。作業中は、エディターに **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はありません。Copilot の処理が終わると、すべての変更に対して **Keep** または **Undo** が表示されます。

7. Copilot に促されたら **Continue** を選択して test を実行します。

   ![Copilot Chat ペインで test の実行確認が表示された画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり test が失敗したりすることがありますが、問題ありません。Copilot は task が完了し、エラーがないと判断するまでコード生成と test を繰り返します。

   ![Copilot Agent Mode の Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI tool が生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新しい機能を確認します。
11. すべてが動作し、コードのレビューも終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI と data layer にわたる新機能を実装する方法
- Copilot Agent Mode がプロジェクトを調べ、関連 file を特定し、複数の変更をまとめて行う方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更と test をレビューする方法

次は、[Playwright MCP server で機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加演習 — ページ分割を実装する

ゲームの一覧が増えるにつれて、ページ分割が必要になります。この演習で学んだことを使い、Copilot にサイトのページ分割を実装してもらいましょう。次の点を検討してください。

- 既存の instruction file を使うなど、現在のベストプラクティスに従う。
- ページサイズを利用者が選択できるようにするか、固定にするかを決める。
- 希望する実装に必要なガイダンスを prompt に含める。
- Copilot にコンテキストを伝えて変更を依頼し、何度か繰り返し作業する場合がある。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent mode: それぞれの機能と使い分け][choose-mode]
- [VS Code の Agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
