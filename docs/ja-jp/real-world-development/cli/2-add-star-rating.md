---
title: "レッスン 2 - 星評価を追加する: 小さな改善から始める"
description: "Copilot CLI でゲームカードを小さく変更し、ポートフォワーディングしたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を始めます。
- プロジェクトに小さな変更を加えるようエージェントに依頼します。
- `/diff` で変更をレビューします。
- アプリを起動し、ポートフォワーディングしたブラウザーで変更を確認します。
- 最初のプルリクエストを開いてマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。しかし、ホームページのゲームカードに表示されるのは、タイトル、カテゴリー、販売元、説明だけです。ウォームアップとして、各カードに既存の評価を表示させます。これは小規模で独立した変更なので、最初のセッションにぴったりです。

## 会話の構成

**会話**では、タスクについて Copilot CLI と一緒に作業します。Copilot app とは異なり、通常の CLI の会話では、専用の worktree を作らずに、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使用します。保存した会話を再開しても、ファイルとブランチはディスク上の通常の Git 状態として残ります。

会話では、プロンプトとエージェントの応答、ファイルを調べて編集するエージェントの tool 操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

新しい会話を始めて、機能の実装に取りかかりましょう。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
3. まだ Copilot が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を使い、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを特定して、必要なコードを作成します。Copilot CLI を使って新しい機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージする前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべての file を確認します。
2. ゲームカードに、評価がある場合は数値、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストで確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含め、生成 AI tool の動作は確率的です。そのため、生成されるコードは毎回異なる場合があります。特定の実装と完全に一致することではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces でポートフォワーディングしたブラウザーから UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 各ゲームカードに 5 段階評価が表示されていることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、プロジェクトにコードをマージするためのプルリクエスト（PR）を作成します。

1. 既定のエージェントに変更を commit するよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. <kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更された file とチェック結果を確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで Copilot の会話を始めました。
- ゲームカードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、ポートフォワーディングしたブラウザーで星評価を確認しました。
- 最初のプルリクエストを開いてマージしました。

次は、[フィルタリング issue から始め、Plan と Autopilot mode を使って][next-lesson]より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
