---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースを調べ、変更を提案し、file を編集し、command を実行するなど、自律的に作業します。VS Code の Copilot Chat では、自分のマシン上でリアルタイムに行われる各 tool 呼び出しや file 編集を確認できます。差分を inline で確認し、個々の変更を採用または却下できます。また、エディターを離れずに追加の prompt を送り、作業を改善したり広げたりできます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムに監視します。
- 提案された file の編集を確認して採用します。
- 作業中の agent に追加指示を送り、ハイコントラストに加えてライトモードの切り替え機能を追加します。

## シナリオ

[演習 4][previous-lesson] で使ったアクセシビリティ用 custom agent は、ハイコントラストモードを実装しています。作業中にデザインチームから追加の依頼がありました。既定のテーマが暗すぎると感じる利用者向けに、ライトモードの切り替え機能も追加します。新しい会話を最初から始めずに、進行中のセッションに指示を送り、作業範囲を広げます。

## Copilot の進捗を監視する

Copilot Chat ビューでは、agent が行う各手順を確認できます。調べた file、実行した command、提案した編集が表示されます。file の編集は差分と **Keep** / **Undo** の操作とともに chat 内に表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. agent の作業中は chat をスクロールし、各 tool の操作を確認します。file の読み取りや編集、実行を求められたターミナル command が表示されます。
3. agent が file を編集すると、差分が inline で表示されます。
   - **Keep** を選択すると、変更を採用します。
   - **Undo** を選択すると、変更を元に戻して、agent に別の方法を試させます。
4. chat の上部にある **Files changed** を開くと、このセッション中に agent が変更した file 全体を確認できます。

> [!NOTE]
> ターミナル command の実行など、tool の操作に許可が必要な場合、agent は処理を一時停止して **Allow** を表示します。内容を確認し、実行を許可できる command のみ承認してください。

5. agent がハイコントラストモードの実装を終えるまで待ちます。変更の概要を説明する最後のメッセージが表示されます。

## ライトモードを追加するよう agent に指示する

ハイコントラストモードができたら、同じ会話でライトモードの切り替え機能も追加します。同じ chat session を続けるため、agent は前の作業で変更した component、スタイル、local storage の使い方などのコンテキストを保持しています。

1. Copilot Chat の入力欄に次の追加 prompt を送ります。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. agent は前の作業を調べ、ライトモードに必要な変更を提案します。
3. ハイコントラストモードのときと同じように、変更された file を確認し、差分ごとに **Keep** または **Undo** を使います。
4. agent が望ましくない方向に進んだ場合（たとえば、好みに合わない方法で toggle UI をリファクタリングする場合）は、chat 入力欄の隣にある **Stop** ボタンで停止し、追加の説明を送ります。

> [!TIP]
> 作業中の session に指示を送ると、最初からやり直すより短時間で調整できます。追加 prompt で、変更の依頼（「CSS を簡潔にし、selector の重複ではなく CSS variable を使ってください」）、方針の修正（「toggle を 3 つにせず、ライトモードを既存の設定パネルに加えてください」）、範囲の拡大（「両方の toggle にキーボードショートカットも追加してください」）ができます。

## 変更内容をレビューする

commit する前に、agent が変更したものをまとめて確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更された file の一覧を確認します。Astro component、スタイル、関連 test などが更新されているはずです。
3. 変更された file をいくつか開き、差分を確認します。custom agent のアクセシビリティに関するガイダンスが反映されていることを確認します。ARIA 属性、キーボード操作、semantic HTML、local storage による設定の保存などを確認してください。
4. 問題がなければ **Source Control** パネルで変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat の agent mode を使って、進行中のセッションを監視して誘導しました。file の編集を確認し、変更を採用または却下し、コンテキストを失わずに追加 prompt で作業範囲を広げました。

このレッスンでは、次のことを学びました。

- agent の作業をリアルタイムに監視する
- file の編集を個別に採用または却下する
- 追加 prompt で進行中のセッションを誘導する

[次のレッスン][next-lesson]では、アクセシビリティの作業をプルリクエストとして公開し、このラボでローカルの custom agent が作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode を使う][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
