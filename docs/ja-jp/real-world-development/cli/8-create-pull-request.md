---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、instruction、skill の更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、品質保証（QA）プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト（PR）は自分でマージしましたが、今回は **Agent Merge** にマージのプロセスを任せます。

> [!NOTE]
> 通常は、機能、instruction の更新、skill の更新、QA agent をいくつかの PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質ワークフロー全体を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びます。
- 機能の変更全体と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローを通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージを許可する権限は開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題に対応します。継続的インテグレーション（CI）チェックの失敗を修正し、レビューコメントに対応し、必要に応じて rebase します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューし、マージできる状態であると判断してください。

## Agent Merge で PR を管理する

コードが揃ったので、まとめてレビューして PR を作成し、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力して agent の選択画面を開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべてのチェックが成功すると、マージを実行します。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されていればレビュー後に手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発作業のいくつかを自動化しました。具体的には、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びました。
- 機能の変更全体と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に役立つ [Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
