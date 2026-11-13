---
title: "レッスン 4 - custom instruction で Copilot をガイドする"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合は、そのガイダンスを Copilot に伝えます。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も指定できます。フィルタリングを実装したので、Copilot が参照した instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位の instruction file がエージェントに適用される仕組みを確認します。
- コーディング標準が守られるように instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、優れた開発チームと同じように、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードの内容を言い換えるのではなく、意図や自明でない決定を説明します。
- `db/` と `src/lib/` にある export された関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro component では `Props` の契約を説明し、関連コードが変わったらコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

Instruction file を使うと、Copilot がこれらのプラクティスに沿って作業できるよう、必要な情報を伝えられます。

## Instruction file

Custom instruction を使ってコンテキストや設定を Copilot に伝えると、コーディングスタイルや要件をより正確に理解させられます。これは提案やコードスニペットをより適切なものにするための強力な機能です。使用するコーディング規約やライブラリ、コメントの種類などを指定できます。リポジトリ全体に適用する instruction も、特定のファイル種別に適用する instruction も作成できます。

Instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載するファイルです。

> [!NOTE]
> ほかの instruction 形式への対応状況は、環境によって異なります。特定の形式を使う前に、[custom instruction の対応状況][custom-instructions-support] を確認してください。

## このプロジェクトの custom instruction file を確認する

作業を始めやすいよう、starter project にはいくつかの instruction file が用意されています。変更を加える前に、既存のファイルを確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディングガイダンスを確認します。この instruction は、このリポジトリに関する Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro file、Drizzle data layer、test などの instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、instruction の適用対象を決める glob が指定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクトファイルへのリンクを確認します。大きな instruction セットを小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルはよい出発点ですが、追加できるルールがあります。中核となる `copilot-instructions.md` を編集し、新しく生成する TypeScript に TSDoc comment が追加されるようにしましょう。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しのセクションを探します。35 行目付近にあります。
2. 次の内容をセクションの最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。instruction を保存した後、フィルタリングの会話を終了して再開してください。機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示された場合は、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript file を確認します。新しい TSDoc comment が追加され、コードを正確に説明していることを確かめます。

## まとめと次のステップ

Copilot CLI が instruction を参照する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの instruction とパス単位の instruction file がエージェントに適用される仕組みを確認しました。
- instruction file を更新して、コーディング標準が守られるようにしました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、lint と test が一貫して実行されるようにします。

## リソース

- [Copilot CLI に custom instruction を追加する][instruction-files]
- [Custom instruction の対応状況][custom-instructions-support]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
