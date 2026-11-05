---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果のレポート形式をカスタマイズしてフィルタリングを検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、開発作業の一部にすぎません。これまではコードが動作することを手動で検証し、命令ファイルを使ってチームの標準に従うようにしてきました。テストや lint、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースがまとめられており、エージェントは必要に応じて読み込みます。スキルの基本は、スキル名のフォルダーと、その中の Markdown ファイル `SKILL.md` です。Markdown には、スキルの名前と説明を定義する frontmatter、概要、呼び出すタイミングのガイダンスが含まれます。フォルダーには、スクリプトやその他のリソースを含むサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有、再利用するリポジトリ資産としてプロジェクトの `.github/skills` フォルダーに配置できます。また、ユーザー スキル用フォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェック用に作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭にある `name` と `description` を読みます。説明によって Copilot がスキルを呼び出すタイミングを判断できることを確認します。
3. 命令を読み、テストと lint の手順をどのように案内しているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更を加える前にスキルを実行する

まずスキルを実行し、ユニット テスト、lint、型チェックの結果を確認します。

1. Interactive mode でフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   quality-checks スキルを実行して、ユニット テスト、lint、型チェックを行ってください。
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容がわかるレポートに改善します。スキルにその形式を指定します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## 結果のレポート形式

   完了後、実行した各コマンドと、その結果が成功、失敗、またはブロックされたかを報告します。テスト件数、所要時間、エラー、警告、その他の指標は、ツールが報告した場合にのみ含めます。失敗やブロッカーがある場合は次の対応を示し、スキップまたは未完了のチェックを成功として報告しないでください。
   ```

3. ファイルが自動的に保存されることを確認します。

## 更新したスキルを実行する

Copilot CLI は、会話を再起動せずに編集したスキルを再読み込みできます。更新したスキルで同じチェックを実行し、レポートを比較します。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と同じプロンプトを使います。

   ```plaintext
   quality-checks スキルを実行して、ユニット テスト、lint、型チェックを行ってください。
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルタリング機能の PR に含まれます。次は、[Playwright MCP server][next-lesson]を使って Copilot がサイトを直接操作できるようにします。

## スキルの例

次のコミュニティの例は参考資料であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート用スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
