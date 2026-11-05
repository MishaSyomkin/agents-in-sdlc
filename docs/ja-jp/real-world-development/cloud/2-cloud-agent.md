---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

多くの組織では、技術的負債に悩まされています。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能などがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、開発者がレビューできるドラフト PR を作成します。時間のかかる作業を委任することで開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 明確な範囲の issue が重要な理由
- issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを十分に残していません。まず、アプリケーションのエクスポート関数すべてに TSDoc の doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示します。現時点では、UX の受け入れテストに使える簡易的な実装でかまいません。この作業は現在のブロッカーですが、より優先度の高い issue もあります。

いずれも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が別の作業を進めている間に非同期で処理でき、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]などの方法で作業を依頼できます。割り当て後、Copilot は進捗を追跡するドラフト プルリクエスト (PR) を作成し、環境を準備してタスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、PR で開発者に通知します。

## 適切な範囲の命令とタスクが重要な理由

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent のタスク範囲を決めるときは注意してください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と Copilot への影響を確認しました。カスタム命令の演習をすでに完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。未完了の場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認し、後から影響を確認できるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` ファイルに命令を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てます。生成された PR で、これらの命令に由来する TSDoc の doc comment、TypeScript の規約、コメント ヘッダーを確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、必要な開発ツールと依存関係を整える環境設定が必要です。SWE と同じように作業を進める Copilot にタスクを割り当てる場合も同様です。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるようになります。このラボのために、以下のワークフローはあらかじめ設定されています。単一の Astro アプリのため、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、移行と seed を実行した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じですが、重要な点が 2 つあります。

- `copilot-setup-steps` という単一のジョブがあります。Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブから手動で実行できるため、Copilot の実行を待たずにワークフローをテストできます。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが理解していますが、情報が古かったり不足していたりするプロジェクトは少なくありません。こうした技術的負債は対応されないままになり、開発の速度を落とし、コードの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成に役立つため、この issue を cloud agent に割り当てます。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの、新しい PR を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は環境を作成し、前述のワークフローを実行し、プロジェクトを分析して、作業の進め方を決めています。

13. todo リストを確認し、Copilot が実行するタスクを読みます。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるように、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールして、Copilot の作業を確認します。コードを調査して状態を理解し、方針を検討してからコードを作成する様子を確認できます。

作業には数分かかることがあります。Copilot cloud agent の主な利点は、ほかの作業を進めている間に非同期でタスクを実行できることです。もう 1 つのタスクを cloud agent に割り当てたら、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent に作業を分けて任せれば、開発者は別のタスクに集中できます。デザイン チーム向けに関連ゲームを追加する作業を、cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す 👀 が issue の最初のコメントに表示されます。

![Copilot が issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent は SWE と同じように動作するため、常に監視する必要はありません。作業が終わったらレビューします。その間に、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使うと、issue を Copilot に割り当て、非同期に作業させられます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに利用できます。

このレッスンでは、次のことを確認しました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 明確な範囲の issue が重要な理由
- issue を Copilot に割り当てる方法

Cloud agent がバックグラウンドで作業する間に、次は[カスタム エージェントを作成して使います][next-lesson]。[Copilot cloud agent は MCP server も利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
