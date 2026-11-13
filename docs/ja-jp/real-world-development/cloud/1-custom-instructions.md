---
title: "演習 1 - Custom instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で進める必要があるタスクや、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにしておきます。[Instruction file][instruction-files] を使うと、Copilot にガイダンスを伝え、何をするかだけでなく、どのように進めるかも理解させられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位で適用される instruction file を使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムで並べて効果を示すのが難しいからです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、instruction file の影響を確認します。

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

## 新しいリポジトリ標準を追加する

この演習で編集するのは、プロジェクト全体に適用するルールを 1 つ追加する作業です。コード内のドキュメントを TSDoc の doc comment とファイルレベルのコメントヘッダーで記述するよう標準を追加します。後の演習で cloud agent に issue を割り当てると、この標準が適用されます。

編集の前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから instruction を読み込みますが、変更をきちんと commit するため、ここではブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## instruction の更新を commit、push、マージする

Cloud agent は issue が対象とするブランチから instruction file を読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が新しい instruction を使えるように、変更を `main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で issue を割り当てると、cloud agent はこの instruction を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略し、`main` に直接 commit できます。ブランチを使う手順は、実際のプロジェクトでの作業方法に合わせています。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューするときに、この標準が反映されていることを確認できます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Copilot の custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
