---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、次の学習に役立つリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されていたフィルタリング issue を見つけました。
- 星評価を追加し、ポートフォワーディングしたブラウザーで結果を確認して、最初のプルリクエスト（PR）を手動でマージしました。
- フィルタリング issue から始め、Plan mode で方針を決め、Autopilot mode で実装し、Interactive mode でレビューしました。
- custom instruction でエージェントをガイドし、既存の `quality-checks` skill をカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol（MCP）server を追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証（QA）用の custom agent を作成して選択し、要件、テスト範囲、skill の結果、ブラウザー上の根拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR の Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュコマンドを確認しました。

## 作成したもの

ワークショップでは、更新済みの `main` からブランチを作成し、次の 2 つの PR を行います。

1. **星評価**: ゲームカードに、既存の `starRating` と未評価であることを明示する表示を追加します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

ワークショップを効率よく進めるため、フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` skill はユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、それらの結果と要件およびテスト範囲のレビューをまとめました。

追加するテストは、実際の不足を補うものであるべきです。QA を実行しても新しいテストが必要ない場合、それが正しい結果であることもあります。マージを許可する前にコードと根拠をレビューし、変更後は影響を受ける根拠を再確認してください。

## ベストプラクティス

Copilot に与えるコンテキストと tool が、その作業内容を左右します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP server を設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。instruction は標準を設定し、skill は繰り返し実行できるタスクをまとめ、custom agent は専門家の役割を定義し、MCP server は外部の tool に接続します。エージェントの要約だけでなく、実際の変更と tool の結果をレビューしてください。

タスクに合った **mode と model** を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は範囲を絞った変更の確認を続けながら進めるとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。定型的な編集には高速な model を、複雑な作業にはより高性能な model を選びます。

ツールの基盤と同じくらい、コンテキストも重要です。*何を*作りたいか、*なぜ*必要か、*どのように*作るかを明確に伝えることで、結果は大きく変わります。

## さらに試す

主要なワークフローを一通り確認しました。Copilot CLI の機能をさらに試すには、次のコマンドが役立ちます。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで上達します。実際のコード、趣味のコード、作ろうと思いながら何年も手をつけていない小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry] に進んでください。別の環境も比べたい場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud] を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
