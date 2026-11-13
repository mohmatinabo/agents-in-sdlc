---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフローにある特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown file を作成して agent を定義し、目的に沿った instruction、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用できる agent にまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディングを促す security agent、一貫したテストパターンを保つ testing agent などです。

Custom agent は、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には `name` と `description` を含む YAML frontmatter を記述し、その後に agent の動作、専門知識、instruction を定義する Markdown prompt を記述します。

### Custom agent と agent skill の違い

Custom agent と [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown file で定義され、AI に作業方法を伝えます。簡潔に区別するなら、**custom agent は作業者、skill は tool** です。

Custom agent は独自のコンテキストウィンドウを持ち、作業の一部として skill（場合によってはほかの agent）を連携して実行します。このラボでは、アクセシビリティ用の custom agent がガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストのワークフロー用 skill やテストを実行・管理する skill などを呼び出すこともできます。

> [!NOTE]
> Custom agent の作成に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべての利用者がクラウドファンディングのプラットフォームを使えるようにしたいと考えています。最近の利用者のフィードバックによると、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいという意見があります。このアクセシビリティの課題に対処するため、デザインチームは利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装する必要があります。custom agent に機能を生成してもらいましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ用 custom agent を確認します。
- Copilot Chat でアクセシビリティ agent を使い、ハイコントラストモードを実装します。

## アクセシビリティ用 custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。Copilot にどのようなガイダンスを与えるか確認しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します（演習 3 のフィルタリング PR とは分けて作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は custom agent に必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat で custom agent を使う

VS Code の Copilot Chat ビュー下部にある agent dropdown には、`.github/agents` に定義されたすべての custom agent が表示されます。custom agent を選択すると、その agent 専用の instruction と tool を使う chat session を開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。mode と model の選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。まだ選択されていない場合は選択してください。

   ![Chat ビューの agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にある agent dropdown を選択します（選択中の agent が表示されます。既定では **default** です）。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. アクセシビリティ agent に次の prompt を送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトの file に適用します。各編集は file のパスと差分とともに chat に表示されます。
6. 編集が適用されると、agent が変更した file を示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は Astro component、CSS、関連 test などの既存 file を変更します。

次の演習では、作業中のセッションを確認して、agent に追加の指示を送ります。

## まとめと次のステップ

GitHub Copilot の [custom agent][custom-agents] を確認しました。Custom agent は特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できる agent にまとめ、Copilot がより効果的に作業できるようにします。

次のことを学びました。

- custom agent の定義方法
- Copilot Chat の agent mode で custom agent を使う方法

次は[agent の作業を監視して誘導します][next-lesson]。変更を確認し、同じセッションにライトモードの切り替え機能を追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の設定][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
