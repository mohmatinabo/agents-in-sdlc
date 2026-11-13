---
title: "演習 1 - Custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で進める必要があるタスクや、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにしておきます。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのは [instruction file][instruction-files] です。コード生成に関するガイダンスを伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位で適用される instruction file を使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- *現在の* instruction を使って、フィルタリングに必要な最初のデータスライス（publishers helper）を作成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい標準が反映されることを確認します。
- instruction の更新とフィルタリングの最初のデータスライスを `main` に commit し、次の演習で新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが、設定した標準から外れる場合があります。Copilot の動作は非決定的です。この演習の目的は、instruction を更新した後の動作の*傾向*を確認することであり、出力を 1 文字ずつ一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、優れた開発チームと同じように、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- data layer には常に unit test が必要です。
- UI はダークモードで、モダンな印象にする必要があります。
- コード内のドキュメントは、TSDoc の doc comment として記述します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

Instruction file を使うと、Copilot がこれらのプラクティスに沿って作業できるよう、必要な情報を伝えられます。

### Custom instruction

Custom instruction を使ってコンテキストや設定を Copilot に伝えると、コーディングスタイルや要件をより正確に理解させられます。これは Copilot からより適切な提案やコードスニペットを得るための強力な機能です。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体に適用する instruction も、特定のファイル種別に適用する instruction も作成できます。

Instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。使用する技術スタック、作成しているものの概要、ベストプラクティスなど、ほとんどの chat や CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語別ガイドラインや、UI component の作成、unit test の追加など、特定のタスク向けのガイダンスを記載します。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの instruction file と、`applyTo` frontmatter がある `*.instructions.md` file の両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準に基づく instruction file][custom-instructions-support] にも対応しています。

### Instruction file を管理するためのベストプラクティス

Instruction file の作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトに含まれる例は、一般的な方法を示しています。基本的には、次のようにします。

- `copilot-instructions.md` は、作成するものの説明、プロジェクト構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスに集中させます。
- `*.instructions.md` file は、unit test、Astro component、data layer など、ファイル種別や特定のタスクに関する具体的なガイダンスに使います。
- 自然な言葉で、明確にガイダンスを書きます。コードの望ましい例と望ましくない例を示します。

Instruction file の作成方法にも、AI の使い方にも、唯一の正解はありません。プロジェクトに合う方法は、試行錯誤しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions] や [Astro][astro-instructions] など、さまざまなタスク向けの file があります。
>
> Copilot に instruction file を作成してもらうこともできます。操作方法は利用環境によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init`）。詳細が関係する箇所では、各環境のレッスンで説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot] には instruction file、custom agent、その他のリソースが集まっています。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに用意されている instruction file を読んでみましょう。中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` file があります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらの内容は Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro file、Drizzle data layer、test などの instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、適用対象を決める glob パターンが指定されています。パターンは repo root からの相対パスで、たとえば `**/*.test.ts` に一致する TypeScript test file が対象です。
5. このプロジェクトの unit test 作成に特化した instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などのほかの instruction file や、プロジェクト内の既存 file へのリンクを確認します。これにより、大きな instruction セットを小さく再利用しやすい file に分け、Copilot にコード生成の参考例を示せます。（記載されているパスは repo root ではなく、instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイルコメントヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成しましょう。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。mode と model の選択は演習によって異なります。必要な場合は、各手順で説明します。

Custom instruction の効果を確認するため、まず現在の instruction を使ってプロンプトを送信します。その後、instruction を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で使ったエディタータブをすべて閉じ、Copilot に必要なコンテキストだけが渡るようにします。
2. `src/lib/publishers.ts` を開き、helper を作成する場所を Copilot に示します。
3. Chat ビューの agent picker で **Agent** を選択し、Copilot が file を編集できるようにします。

   ![Chat ビューの agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認してコードを更新します。多くの場合、helper file と test の両方が変更されます。
6. 提案された helper が型付き関数で、最初の引数として `db` client を受け取り、型付きの publishers 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の data layer 規約によるものです。
7. 提案されたコードに TSDoc の doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。この演習の目的は、instruction の更新後に一貫性が向上することを確認することです。

## 新しいリポジトリ標準を追加する

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えるための file です。コードの提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはまだありません。そのため、生成された helper に doc comment がありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、Copilot にファイルコメントヘッダーと TSDoc の doc comment を追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、instruction file は全体的なガイダンス用のリポジトリレベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。ここで追加した doc comment ルールのように、プロジェクト全体に適用する標準はリポジトリレベルの file に記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象 file に注目するようにします。
3. 先ほどと**同じ prompt** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案された file の先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment が含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を誘導して新しいプロジェクト標準に従わせ、次の演習で土台として使うコードに適用できました。

## instruction を commit してブランチを push する

Instruction file もリポジトリ内のほかの file と同じように、ソース管理で管理します。変更を commit してブランチを push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押し、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、instruction と helper の変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで instruction の更新と helper の土台に関する file を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルでブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、プルリクエストを作成し、バックログのドキュメント標準に関する issue と関連付けます。手動で行うこともできますが、プロジェクトに接続済みの GitHub tool を使って Copilot に依頼できます。issue を探し、issue をクローズする PR を作成してからマージするよう prompt を送ります。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、今回が初めてです。プロジェクトテンプレートでは GitHub tool がすでに workspace に接続されています。Copilot が初めて tool を使うときに、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って認証してください。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file の更新に関する issue を探し、instruction の変更と新しい publishers helper の土台の両方を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認して適切であれば **Allow** を選択します。
6. PR が作成されたら、次の prompt で Copilot に PR をマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作や shell command の実行許可を求めたら、コマンドを確認して適切であれば **Allow** を選択します。

GitHub Copilot の助けを借りて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の instruction を使ってコード生成 prompt を送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じ prompt をもう一度実行し、新しい標準が提案コードに反映されることを確認しました。
- instruction の更新と helper の土台を `main` に commit し、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに [agent mode で instruction を活用します][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Copilot の custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
