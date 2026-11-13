---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub 上で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べて変更を行い、プルリクエストを作成します。その間、ほかの作業を続けられます。

各演習では、まず cloud agent が従う custom instruction を追加し、GitHub issue を割り当てて実装を依頼します。custom agent を確認して使い、進め方を調整しながら、agents ダッシュボードでセッションを監視、誘導します。最後に、エージェントが作成したプルリクエストをレビューして、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Custom instruction][ex1] | コンテキスト | cloud agent が従う custom instruction を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を GitHub Copilot cloud agent に割り当てる |
| [3. Custom agent][ex3] | 特化型エージェント | custom agent を確認して使う |
| [4. エージェントを管理する][ex4] | 監視 | エージェントセッションを監視して誘導する |
| [5. 反復して改善する][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を改善して、次のステップを選ぶ |

## 前提条件

ワークショップに参加する前に、次の準備が必要です。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランのいずれかがある GitHub アカウント
- [ ] アカウントまたは組織で GitHub Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していませんか。対象となる学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Free 以外のすべてのプラン**（Copilot Student、Pro、Pro+、Business、Enterprise）で利用できます。Copilot Business または Enterprise の場合、組織の管理者が有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP server を使う方法については、[CLI ハーネス](../cli/) または [VS Code ハーネス](../vscode/) を参照してください。

## 始める

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
