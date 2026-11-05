---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。アプリを手動で操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP server が新しいツールで Copilot を拡張する方法を学びます。
- Playwright MCP server を workspace に追加します。
- Copilot にブラウザーでフィルタリング機能を手動テストしてもらいます。
- ブランチを公開し、フィルタリングの PR を作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にある機能にアクセスできると、さらに便利になります。Copilot が外部のツールやサービスと通信するための標準的な方法が Model Context Protocol (MCP) です。

![agent mode の仕組みと、コンテキスト、LLM、ツール（MCP server と VS Code 拡張機能が提供するツールを含む）とのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部ツールやサービスと通信するための仕組みです。MCP を使うことで、エージェントは外部サービスとリアルタイムにやり取りし、最新情報（resources）へのアクセスや操作（tools）を行えます。

ツールやリソースには、AI エージェントと外部サービスの間をつなぐ MCP server を通じてアクセスします。MCP server は、既存の API や NPM package のようなローカル ツールとの通信を管理します。各 server は、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP server の例を次に示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供し、Web ページの移動、フォームの入力、ボタンの選択などを行えます。

ほかにも多くの MCP server が利用できます。GitHub は、エコシステム内の server を見つけやすくし、貢献を促進するために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> MCP server は、プロジェクトのほかの依存関係と同様に扱ってください。利用前にソース コードを注意深く確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP server のみを利用し、機密リソースや操作へのアクセスを許可する際は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code workspace で利用する MCP server を構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` server が設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

`github` の設定はプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が issue のバックログを読み取れました。これは[リモート GitHub MCP server][remote-github-mcp-server]を使います。ローカルへのインストールは不要で、VS Code が HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP server を追加する

次に、2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は Copilot が操作できるブラウザーを提供し、機能のテストに使えます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` server と異なり、`playwright` は**ローカル** server です。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は、画面を表示せずにブラウザーを動かします。デスクトップ画面がない codespace では必須です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストにすでに Playwright を使っているため、必要なブラウザーも通常はインストール済みです。Copilot が後でブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行します。

## server を起動して信頼する

VS Code は MCP server を**必要に応じて**起動します。Copilot が server のツールを初めて必要としたとき、VS Code が server を起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に表示される **Start** 操作を選択し、server を今すぐ起動して構成を確認します。
2. VS Code が server を信頼するか確認した場合は、構成を確認して信頼する設定を選び、server を起動します。

手動で起動する手順は任意です。省略した場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code が起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作する代わりに、エージェントがページを開き、移動してフィルターを適用し、結果を読み取って要約します。会話を離れずに機能の動作を確認できます。

Playwright MCP server はスクリーンショットではなく、ページの [accessibility tree][playwright-mcp-server] を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を扱います。簡単な機能確認が、アクセシビリティの基本的な確認も兼ねます。

server を接続してアプリを起動したら、次のプロンプトを Copilot に送信して、作成したフィルタリング機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件と照らし合わせて要約を読みます。問題がある場合は、質問を続けるか、PR を作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP server を初めて使うとき、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないという報告があれば、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit メッセージを生成します。

   ![変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub command-line interface (CLI) などから PR を作成できます。ここでは、GitHub のツールを使える Copilot に作成を依頼します。関連 issue を見つけて、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. agent dropdown で **Agent** が選択されていることを確認し、Copilot が GitHub のツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作を許可します。GitHub のツールを初めて使用するときは、GitHub で認証するよう求められる場合があります。表示される手順に従ってください。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP server が新しいツールで Copilot を拡張する方法を学びました。
- Playwright MCP server を workspace に追加しました。
- ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリングのプルリクエストを作成しました。

MCP server を使って機能を*テスト*しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティ レビューなどの作業に役立つ[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
