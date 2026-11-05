---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリング機能を確認しました。次は、これらの機能を明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する仕組みを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選び、フィルタリング issue に照らした結果を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫した方法で確認したいと考えています。カスタム エージェントを使うと、この QA プロセスをまとめて、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルには目的、命令、使用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチーム標準を示し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を確認して結果を報告するための機能の使い方を Copilot に示します。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA で行うチェックと守るべき境界の両方を記述します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトでカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA の進め方と制約がプロファイルに含まれていることを確認します。確認しておくことで、検証だけを依頼したのに不完全な設定や広すぎる権限で機能を変更する事態を防げます。

1. `/diff` を実行し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は任意ですが、含めると表示名が明確になります。
3. プロファイルの命令を読み、要件から始めること、リポジトリの命令に従うこと、`quality-checks` スキルを実行すること、Playwright MCP を使うことを確認します。
4. 根拠を示した報告を行い、実装コードを変更する前に確認を求め、変更の commit や PR の作成を行わないことを確認します。
5. 必要な役割や制約が抜けている場合は、続行する前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は、会話を開始するときにプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じフィルタリングの会話を再開し、QA を選択すると、issue と計画で決めた内容を引き継いで確認できます。

1. 次のコマンドで QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能な一覧に表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで、機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 完了したら、エージェントのレポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割を追加し、その作業を確認しました。このレッスンでは、次のことを行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、フィルタリング issue に照らした結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は、[これらを機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
