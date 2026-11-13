---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返しながら進めます。

## シナリオ

あなたは、ボードゲームのクラウドファンディングを提供する架空の会社 Tailspin Toys に加わったばかりの開発者です。チームのバックログは GitHub issue として登録されており、すぐに取り組めます。フィルタリングやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質向上も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。自分に合った環境を選んでください。各ワークショップでは、それぞれ独自のセットアップを行い、共通の Tailspin Toys シナリオに取り組みます。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、instruction、skill、custom agent、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ app で複数のエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業を行い、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP server、custom agent を活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
