---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業を進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調べ、登録済みのフィルタリング issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング issue を起点に、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルを調整して単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選び、要件、テスト範囲、スキルの結果、ブラウザーで確認した証拠を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge を有効にしました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使える slash command を確認しました。

## 提出した変更

このワークショップには、それぞれ更新済みの `main` から作成したブランチを使う 2 つの PR があります。

1. **星評価**: ゲーム カードに既存の `starRating` と、評価がない場合の状態を表示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` のレポート形式をカスタマイズし、QA プロファイルと関連テストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、関連する作業を 1 つの PR にまとめています。

## 検証の種類

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、指定した形式で報告しました。QA は、PR の前にこれらの結果と要件、テスト範囲のレビューをまとめました。

追加するテストは実際の不足を補うものにします。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと根拠を確認し、変更後は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に渡すコンテキストとツールが、作業に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP server を構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し可能なタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP server は外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、範囲を絞った変更に参加しながら取り組むには **Interactive**、範囲が明確な作業を任せるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

ツールと同様に、コンテキストも重要です。何を作るのか、なぜ必要なのか、どう進めるのかを明確に伝えることで、出力が大きく変わります。

## さらに試す

中心的なワークフローを終えました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスク用に独立した Git worktree を作成します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールでも、上達する最善の方法は使い続けることです。本番コード、趣味のコード、何年も考えていた小さなアプリなどで活用してください。学びをチームと共有し、チームの経験からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境を試す場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
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
