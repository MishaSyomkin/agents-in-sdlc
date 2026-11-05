---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、追加の自動化など、開発にはさまざまな作業があります。MCP server を使うと、Copilot CLI に組み込まれている機能を拡張し、より多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使われ方を理解します。
- Playwright MCP server がまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

単体テストやエンドツーエンド テストは大切ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が開発中のサイトをユーザーのように操作できるようにすると、変更が期待どおりに動作するか確認しやすくなります。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信するための仕組みです。MCP を使うことで、エージェントは外部サービスとリアルタイムにやり取りし、最新情報へのアクセスや操作を行えます。

ツールやリソースには、AI エージェントと外部サービスをつなぐ MCP server を通じてアクセスします。各 MCP server は、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP server の例を次に示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP server が利用できます。GitHub は、エコシステム内の server を見つけやすくし、貢献を促進するために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP server は、プロジェクトのほかの依存関係と同様に扱ってください。利用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot がユーザーと同じようにサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP をすでに構成している場合は、このセットアップを飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを実行して、MCP server の追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. server の種類が **STDIO** であることを確認します。<kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP の画面を閉じます。

## Playwright を使って機能を試すよう Copilot に依頼する

先ほどは機能が期待どおりに動作することを手動で確認しました。今度は、Copilot CLI から利用できる Playwright MCP server を使って同様に確認します。

1. 次のプロンプトを使い、Playwright MCP server で機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP server を使うため、明示的に指定する必要はありません。ただし、使用する server が分かっている場合は、プロンプトに含めると結果が安定し、トークンも節約できます。

2. Copilot がブラウザーで実行する手順を確認します。
3. レポートを読み、機能が期待どおりに動作するか確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Playwright MCP server を使い、Copilot CLI から実際のブラウザーで機能を試しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使われ方を学びました。
- Playwright MCP server がまだ利用できない場合は追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次は、スキルとブラウザー ツールを組み合わせる[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
