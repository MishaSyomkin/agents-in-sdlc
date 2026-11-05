---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行し、失敗を調査して不足したファイルを見つけ、さらに変更を加えます。この作業は何度も続くことがあります。

Copilot Agent Mode は、このような作業に役立ちます。

Copilot Agent Mode は IDE 内でより自律的に動作します。開発者のようにプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認します。

> [!NOTE]
> agent mode と cloud agent は名前が似ていますが、異なる用途向けに作られています。Agent mode は IDE 内で作業し、素早くやり取りしながら進めます。Cloud agent はチーム メンバーのように非同期で作業し、issue とプルリクエストを通じて開発者とやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけて、複数のファイルにまたがる変更を行う方法
- Copilot Agent Mode で UI とデータ レイヤーに新機能を実装する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers helper を追加しました。残りのデータ レイヤー、UI、テストの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を把握するため、Tailspin Toys の Web サイトを確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス helper は `src/lib/` にあり、必要なインタラクティブ機能は標準 DOM API を使う小規模な Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを実行すると、単一の Astro 開発サーバーが起動します。GitHub Codespace では次の手順で実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナル内の Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーをホストしている codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、次の主な機能を確認します。

- **Home Page**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加します。まずバックログを確認します。テンプレートからプロジェクトを設定したときに issue が自動作成されているので、GitHub Copilot に表示を依頼します。

1. Chat ビューの agent dropdown で **Agent** を選択します。**Agent** agent はファイルをまたぐ変更を自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。コード提案を比較的一貫させ、良い体験を提供することが目的です。ただし、大規模言語モデル (LLM) は確率的に動作するため、手順に記載したものと異なる提案が返る場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、GitHub リポジトリの issue を表示してもらいます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと考えた項目に基づいて、優先順位も付けたことを確認します。

## 命令ファイルを確認する

コード生成を始める前に、作業のコンテキストを Copilot に渡す命令ファイルを確認します。Web サイトに機能を追加する方法が記載された [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル種別に対応する命令ファイルへのリンク

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルからの相対パスです。命令を再利用し、複雑な内容を扱いやすい単位に分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成するには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する
- フィルタリングの動作に対するテストを追加または更新する
- ゲーム一覧ページにフィルタリング UI を追加する

コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトを使い、バックログの関連 issue に沿って機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、必要なファイルを探す様子を確認します。データ レイヤー helper、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。Copilot の作業が終わると、すべての変更に対する **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが見つからなくなるまで進めます。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加しました。この演習では、次の方法を確認しました。

- Copilot Agent Mode で UI とデータ レイヤーに新機能を実装する
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけて、複数のファイルにまたがる変更を行う
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする

次は、[Playwright MCP server で機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の探索演習 — ページングを実装する

ゲーム一覧が増えるとページングが必要になります。この演習で学んだ方法を使い、Copilot にページングを追加するよう依頼します。次の点を検討してください。

- 命令ファイルの利用など、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかを検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼し、コンテキストを追加しながら反復する必要がある場合があります。これは通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
