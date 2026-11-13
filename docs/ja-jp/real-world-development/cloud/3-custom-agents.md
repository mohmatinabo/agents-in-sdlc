---
title: "演習 3 - Custom agent"
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

Custom agent を使って、次のことを学びます。

- custom agent の定義方法
- custom agent へのタスクの割り当て方

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべての利用者がクラウドファンディングのプラットフォームを使えるようにしたいと考えています。最近の利用者のフィードバックによると、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいという意見があります。このアクセシビリティの課題に対処するため、デザインチームは利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装する必要があります。custom agent に機能を生成してもらいましょう。

## アクセシビリティ用 custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。Copilot にどのようなガイダンスを与えるか確認しましょう。

Codespace に戻り、アクセシビリティ用 custom agent の file を確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は custom agent に必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## issue を作成して割り当てる

Mission control は、環境内のすべての agent を管理する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加のガイダンスを送れます。まず、ハイコントラストモードの作成を Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![Custom agent と Accessibility が強調表示された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

GitHub Copilot の [custom agent][custom-agents] を確認しました。Custom agent は特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できる agent にまとめ、Copilot がより効果的に作業できるようにします。

次のことを学びました。

- custom agent の定義方法
- Copilot へのタスクの割り当て方

Copilot がハイコントラストモードの実装を進めている間に、次は mission control から[エージェントのセッションを監視して誘導します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [組織で custom agent を使うための準備][org-custom-agents]
- [Enterprise で custom agent を使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
