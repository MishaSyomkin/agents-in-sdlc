---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意されたフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールを利用できます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。最初の手順として、使い慣れたツールで Copilot CLI をインストールします。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub Model Context Protocol (MCP) server でフィルタリング issue を見つけます。

## シナリオ

チームは、増え続けるバックログに取り組むため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が普段作業するターミナルでエージェントを利用できるようにします。このレッスンでは、残りのワークショップで使えるようにインストールして認証します。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js がインストール済みのため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たすことを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次の確認が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 使い方を確認する

通常の shell プロンプトでは、コマンドは Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストール済みのバージョンで利用できるコマンドを確認してから <kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の質問で Copilot が動作することを確認します。

   ```plaintext
   このプロジェクトの重要なファイルは何ですか。
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を実行し、組み込みの GitHub MCP server が利用できることを確認します。
6. 次の依頼でフィルタリング issue を探します。

   ```plaintext
   GitHub MCP を使って、このリポジトリで "Allow users to filter games by category and publisher" というタイトルの issue を見つけてください。URL と短い概要を教えてください。何も変更しないでください。
   ```

7. 表示された URL を開き、issue を読みます。最初の小さな変更を完了した後に使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使います。自動的に worktree は作成しません。変更を始める前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は設定済みの権限外でツールを使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動し、Codespace 内でツールの承認確認を省略して演習に集中します。

> [!CAUTION]
> `--yolo` は全自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や仮想マシン (VM) などの分離された環境でのみ使い、日常の開発用の既定値として alias に設定しないでください。詳細は[ツールの許可と拒否][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` は、このワークショップの後のレッスンで issue やプルリクエストを操作するために使う、読み書き両方の GitHub MCP ツールを有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証された GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 会話が動作することを確認するため、プロジェクトについて別の簡単な質問をしてから `/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで命令を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI をインストールして認証しました。このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP server でフィルタリング issue を見つけました。

次は、[最初の対象を絞った変更][next-lesson]に取り組み、ゲーム カードに星評価を表示するよう Copilot CLI に依頼します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
