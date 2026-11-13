---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。Cloud agent は非同期かつ自律的に作業しますが、進行中のタスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する tool は複数あります。その 1 つが GitHub.com の [agents ページ][agents-page] です。この mission control では、open なプルリクエスト（PR）がある agent のタスクを確認できます。実行された操作を調べたり、進行中のセッションに追加のガイダンスを送ったりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼します。

## シナリオ

ハイコントラストモードの作成を agent に依頼した後、チームはライトモードも追加するとよいと考えました。サイトのスタイルと切り替え機能を更新しているため、同時に追加するのが適切です。ハイコントラストに加えてライトモードも実装されるよう、agent の作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents) を開きます。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。割り当てた次のタスクが表示されます。
   - コードベースのドキュメントを更新する
   - ゲームを更新する API を生成する
   - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。たとえば、次のような操作が表示されます。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP server を設定する
   - 割り当てたタスクを完了するための手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される pull request（PR）ペインも確認します。ここから PR と変更された file を確認し、作業をさらに監視できます。

## Cloud agent に作業の方向を伝える

実行中のタスクを確認したので、ハイコントラストモードに取り組んでいる Copilot に、ライトモードの切り替え機能も追加するよう依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状態によって異なります。

   ![Mission control に表示されたアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、agent が作業を開始するまで数分間セッションを確認します。以下のようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに次の prompt を入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents ページで、作業中の cloud agent に指示するダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が prompt を受け取り、作業手順に追加する様子を確認します。

## Copilot の作業を待つ

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼を作業に取り込みます。

完了まで数分かかります。ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って、GitHub Copilot cloud agent のタスクを監視して誘導する方法を確認しました。この mission control では、実行中と完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送れます。

次のことを学びました。

- mission control と agents ページで cloud agent のタスクを監視する
- 進行中のセッションに別の機能を依頼する

Copilot がアクセシビリティ機能を実装している間に、次は [Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
