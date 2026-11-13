---
title: "レッスン 3 - Agent mode: Plan と Autopilot"
description: "Plan で進め方を決め、Autopilot で issue に基づいてフィルタリングを実装し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前のレッスンでは、プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って作業できるため、適切なものを適切な方法で構築できます。このレッスンから、エージェントを活用した一般的な開発プロセスをいくつかのレッスンにわたって進めます。まず issue を使って機能を作成し、コードが有効で期待どおりに動作することを確かめ、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能開発のワークフローでは、同じ会話とブランチを使い続けます。通常は作業対象ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中できるよう簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan mode で要件を決めます。
- Autopilot mode で機能を実装します。
- コードをレビューします。
- ポートフォワーディングしたブラウザーで機能を手動で検証します。

この機能開発を続ける中で、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA agent を作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、利用者はカテゴリーと販売元でゲームを絞り込む必要があります。バックログの issue には機能の概要が記載されていますが、複数のカテゴリーを組み合わせる方法など、実装前に決めておく必要がある点があります。Plan mode で方針を決めてから、Autopilot に範囲を明確にした実装を任せます。

## 背景

AI コーディングエージェントを開発ワークフローに導入しても、基本的な手順は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト（PR）を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものになります。

この標準的な進め方に従うことで、AI が生成したコードも要件を満たし、手作業で書いたコードと同じレビューのプロセスを通せます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。計画中はプロジェクトファイルを編集できません。
- **Autopilot**: エージェントがコードの作成、テストの実行、反復的な修正を自律的に行い、タスクの完了を目指します。

まず Plan mode で計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止中の場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7`（または同様の番号）で始まることを確認してください。`#` を使うと、GitHub 上の issue やプルリクエスト（PR）をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能やタスクを Copilot に任せる前に、実装方法を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan mode に切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について質問されることがあります。希望する仕様に基づいて回答してください。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）を押して計画を展開します。
5. 上下にスクロールして計画を確認します。
6. 自分の決定と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot を使うと、Copilot は問題の解決が完了したと判断するまで作業を繰り返します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様の選択肢を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業を繰り返し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と見なす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同様にマージする前にレビューが必要です。コードを確認し、サイトを実行して問題がないか確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。モード表示に **Plan** や **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力して、フィルタリングの実装とテストを確認します。
3. 結果を issue および計画時の決定と照らし合わせます。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードの確認はできました。次は、実際に動作するか確かめましょう。前と同じようにアプリを起動し、Codespaces のポートフォワーディング経由でサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カテゴリーによる絞り込み、販売元による絞り込み、および計画時に決めた組み合わせを試します。
4. リセット時と結果が空の場合の動作が、issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成しレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan mode で要件を決めました。
- Autopilot mode で機能を実装しました。
- コードをレビューしました。
- ポートフォワーディングしたブラウザーで機能を手動で検証しました。

次は、ドキュメント化されたプラクティスに沿ってコードを生成する方法を詳しく学ぶため、[custom instruction を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
