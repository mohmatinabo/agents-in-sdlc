---
title: "レッスン 5 - 品質チェック skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして、フィルタリングを検証するために使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、コードを書くことだけではありません。手動でコードの動作を確認し、instruction ファイルを使って標準に従うようにしてきました。では、test や lint、継続的インテグレーション（CI）のほかの作業はどうでしょうか。

こうした作業には、**agent skill** が最適です。skill を使うと、test や lint のような操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果の書式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、unit test、lint、type check に `quality-checks` skill を使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## instruction、script、resource

agent skill は、再利用可能なタスクの instruction、実行可能な script、補助リソースをまとめたもので、agent が必要に応じて読み込みます。基本的には、skill 名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown には、skill の名前と説明を定義する frontmatter、skill の概要、呼び出すタイミングのガイダンスが含まれます。skill のフォルダーには、呼び出し時に利用する script やその他のリソースを含むサブフォルダーも追加できます。

> [!NOTE]
> skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにある既存の command を使うため、`SKILL.md` のみで構成されています。

skill は、チームで共有・再利用できるようプロジェクトの `.github/skills` フォルダーに置くことも、ユーザーの skill フォルダー `~/.copilot/skills` に置くこともできます。

## skill を確認する

unit test、lint、type check を実行するために Tailspin Toys チームが作成した `quality-checks` skill を確認しましょう。

1. Codespace に戻り、`.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は Copilot が skill を呼び出すタイミングを判断するのに役立ちます。
3. instruction を読み、test と lint の手順をどのように案内しているか確認します。
4. skill にはまだ **Results output formatting** セクションがないことを確認します。

## 変更前に skill を実行する

skill は Copilot CLI から直接呼び出すことも、自然な言葉で実行を依頼することもできます。3 つのチェックを実行するよう Copilot に依頼しましょう。

1. Interactive mode のフィルタリング会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容を確認できる、より良いレポートにしましょう。結果を報告するためのセクションを skill に追加します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更を加えた skill を Copilot CLI は会話を再起動せずに再読み込みできます。動作を確認しましょう。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 表示されたレポートを確認し、最初のレポートと比べます。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- unit test、lint、type check 用の `quality-checks` skill を確認しました。
- 結果の書式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は、機能のプルリクエストにフィルタリング機能とともに含めます。次は [Playwright MCP server][next-lesson] を使って、Copilot がサイトを直接操作できるようにします。

## skill の例

以下はコミュニティによる参考例であり、このレッスンの追加課題ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図と同梱の export script: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
