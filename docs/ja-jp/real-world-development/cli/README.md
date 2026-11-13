---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI の mode、カスタマイズ、MCP tool、プルリクエスト自動化を学びながら、Tailspin Toys で 2 つの変更を構築、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型コーディングアシスタントとして利用できます。コードベースを調べ、コードを生成し、コマンドを実行して、外部 tool に接続できます。コマンドラインだけで作業できるため、グラフィカルエディターに切り替えずに作業の流れを保てます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って作業します。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトを確認します。
2. 星評価の小さな変更を行い、ブラウザーでレビューして、最初のプルリクエスト（PR）を手動でマージします。
3. フィルタリング issue から始め、Plan mode で進め方を決め、Autopilot mode で実装し、Interactive mode でレビューします。
4. リポジトリの instruction を更新し、フィルタリング機能に適用します。
5. 既存の `quality-checks` skill をカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol（MCP）server を追加して、ブラウザーでフィルタリングを確認します。
7. quality assurance（QA）用の custom agent を作成し、要件、テスト範囲、検証の根拠をレビューします。
8. フィルタリングの変更全体をレビューし、フィルタリング PR に Agent Merge を使います。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認します。

ワークショップに集中できるよう、PR は 2 つ作成します。1 つ目は星評価、2 つ目は instruction の更新、skill の更新、QA プロファイル、テストを含むフィルタリング機能です。フィルタリングと品質ワークフローは、会話とブランチを 1 つにまとめ、各 tool を試しながら作業を積み重ねます。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. Copilot CLI をインストールする][ex1] | インストール | Copilot CLI をインストールして認証し、プロジェクトを確認する |
| [2. 星評価を追加する: 小さな改善から始める][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. Agent mode: Plan と Autopilot][ex3] | Agent mode | issue から機能を計画し、Autopilot で構築して、Interactive mode でレビューする |
| [4. Custom instruction で Copilot をガイドする][ex4] | コンテキスト | instruction を確認して更新し、フィルタリング機能に適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存の skill を確認してレポート形式を変更し、実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | CLI で MCP を設定し、フィルタリングの動作を確認する |
| [7. QA agent を作成して使う][ex7] | 要件とテスト範囲 | 専門のプロファイルを作成して選択し、最終確認の根拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、次の学習に役立つリソースを確認する |
| [任意: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づく agent をデプロイして Web サイトに接続する |

## 前提条件

ワークショップに参加する前に、次の準備が必要です。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランのいずれかがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランを利用していませんか。対象となる学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## 始める

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
