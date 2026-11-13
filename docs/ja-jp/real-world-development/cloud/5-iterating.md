---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的に、GitHub Copilot にいくつかのタスクを依頼しました。コードへのドキュメント追加、デザインチームが改善できる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も確認したように、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視を受けずに作業します。安全に作業できるよう、次のようなセキュリティ上の制約があります。

- Copilot はリポジトリに対する読み取りアクセスを持ち、書き込みアクセスは作業に使うブランチ**のみ**に制限されます。
- Cloud agent は GitHub Actions 内で実行され、作業用の一時的な環境を別途作成します。
- GitHub Actions workflow は、実行前に人間の承認が必要です。
- MCP server を含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードへのドキュメント追加に関するプルリクエスト（PR）を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR に GitHub Copilot が firewall によってブロックされているという警告が表示される場合があります。Copilot は MCP server などの外部リソースに既定では制限付きでアクセスするため、これは**想定どおり**です。必要であれば、[Copilot cloud agent の firewall をカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。その場合は Copilot の作業が終わるまで待ってください。数分かかることがあるため、休憩したり、これまでに学んだ内容を振り返ったりしてもかまいません。

4. プルリクエストの準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しい TSDoc の doc comment など、更新されたコードとドキュメントを確認します。実際の変更内容は状況によって異なります。

   変更を確認するときは、TSDoc の doc comment、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認した custom instruction file によるものです。

6. 更新内容をレビューし問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. workflow の実行承認が必要であることを示す表示を探します。
8. 承認待ちの workflow があれば、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストの checks セクションに workflow が登録されます。問題がなければ、単一 Astro app のプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは、一方通行ではありません。チームメンバーと同じように、プルリクエストやコードの inline comment で Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は毎回異なるため、必ず依頼する内容を 1 つに限定する必要はありません。たとえば、次のような更新を依頼できます。

- 各コード file の先頭に、file の役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro file に TSDoc の doc comment を追加する。
- Astro app の構造を説明する README を作成する。

1. 上記の例などを参考に、コードベースに追加したいドキュメントの変更をコメントで依頼し、通常のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しい commit が追加されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで Copilot に修正を依頼できます。満足したら、draft PR をレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. workflow の実行承認が必要であることを示す表示を探します。
7. 承認待ちの workflow があれば、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストの checks セクションに workflow が登録されます。問題がなければ、単一 Astro app のプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えてください。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に修正を依頼して、繰り返し改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc の doc comment を追加したりできます（この作業は custom instruction を更新する**前**に割り当てました）。前の PR と同様に、**Conversation** タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用 custom agent で実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。その場合は作業が終わるまで待ってください。完了まで数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI component
   - ユーザー設定を保存する local storage の使い方
   - ハイコントラストモードとライトモードの CSS やスタイル変更
   - アクセシビリティ属性（ARIA label、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript code

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. workflow の実行承認が必要であることを示す表示を探します。
8. 承認待ちの workflow があれば、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストの checks セクションに workflow が登録されます。問題がなければ、単一 Astro app のプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えます。ページを再読み込みしても設定が保存されることを確認してください。

アクセシビリティ用 custom agent が、Copilot にアクセシビリティのベストプラクティスに沿った機能を実装するよう促したことが分かります。アクセシビリティに関する懸念や改善点があれば、前の PR と同じように **@copilot** をコメントでメンションし、更新を依頼できます。

## 任意の演習 — さらに作業を委任する

Cloud agent は、実際のバックログ項目を渡して別の作業に集中できるときに効果を発揮します。練習として、リポジトリにいくつか issue を作り、Copilot に割り当ててみましょう。次のような作業が考えられます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- data-access helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **このリポジトリに用意された custom instruction file を確認し、cloud agent の出力に反映される内容を把握しました。**
- **GitHub issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。**
- **アクセシビリティ用 custom agent を作成して使い、ハイコントラストとライトモードの切り替えを追加しました。**
- **Copilot agents ページを mission control として使い、実行中のアクセシビリティセッションを監視して誘導しました。**
- **cloud agent のプルリクエストをレビューして改善し、変更を依頼するために `@copilot` をメンションし、workflow を承認しました。**

## レビューと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ほかのハーネスも今回の内容を補完します。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot mode、agent skill、QA custom agent、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のような作業を試してください。

- リファクタリングやテスト範囲の拡大に関する issue を cloud agent に割り当てる。
- 別の分野向けの custom agent を作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow に関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent firewall の設定][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
