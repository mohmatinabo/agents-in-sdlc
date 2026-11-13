---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意されたフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディングアシスタントです。コマンドラインからコードベースを調べ、コードを生成し、コマンドを実行し、外部ツールと連携できます。タスクを任せたり、変更を依頼したりしながら、集中して作業できます。最初のステップはツールのインストールです。使い慣れたツールを使って簡単にインストールできます。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP server を使って、フィルタリング issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI agent を使い始めています。Copilot CLI は多くの開発者が日常的に使うターミナルに、その機能をもたらします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初めて起動すると、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められたら、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次の prompt が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼を保存します。

> [!NOTE]
> Codespace では GitHub のセッションを通じて認証済みの場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常の shell プロンプトでは、コマンドは Codespace で直接実行されます。Copilot CLI を起動すると、自然な言葉は agent に送られ、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。確認のため、もう一度 <kbd>Enter</kbd> を押します。
2. `/help` を入力して、インストール済みのバージョンで利用できる command を確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、正しく動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べていることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP server が利用できることを確認します。
6. Copilot にフィルタリング issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、issue の内容を読みます。最初の小さな変更を終えた後に、この issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree は作成されません。変更を始める前に、機能ブランチを作成します。

## ワークショップ用の shortcut を使う

通常、Copilot CLI は許可済みの範囲外で tool を使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。Codespace 内でこの shortcut を使うと、承認の確認を省略して演習に集中できます。

> [!CAUTION]
> `--yolo` は完全な自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM などの分離された環境でのみ使い、日常的な開発の既定値として alias に設定しないでください。詳しくは [Allowing and denying tool use][allow-all-warning] を参照してください。

`--enable-all-github-mcp-tools` を指定すると、後のレッスンで issue やプルリクエストに使う、読み取りと書き込みの GitHub MCP ツールが有効になります。Codespace によりローカルコンピューターへのアクセスは制限されていますが、認証された GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう 1 つ簡単な質問を送り、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で instruction を変更したり agent を追加したりしたときに、`copilot --resume` で同じ機能の会話とブランチに戻れます。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP server を使って、フィルタリング issue を見つけました。

次は[最初の変更に取り組み][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
