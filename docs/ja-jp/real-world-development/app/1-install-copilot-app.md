---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

[GitHub Copilot app][about-copilot-app] は、エージェント主導の開発に使うデスクトップ アプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントをそれぞれ独立した workspace で並行して動かし、作業を指示するワークフローに適しています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートで用意されたバックログなど、workspace の使い方を確認します。
- クイックチャットでアプリについて質問します。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app を使うと、issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始めます。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン（Pro、Pro+、Business、Enterprise）を利用してください。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使用するには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用するプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地が設定された GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディング中にリポジトリを選択する画面が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選びます。リポジトリの選択をスキップし、オンボーディング後にプロジェクトを追加することもできます。
6. テーマを選択するよう求められたら選び、オンボーディングを完了します。
7. サイドバーの **Sessions** の下にリポジトリが表示されていることを確認します。すでに一覧にある場合は、もう一度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた owner と repository の名前を使います。
9. サイドバーにリポジトリが表示されることを確認します。このワークショップで使うレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にグループ化され、その下に個々のセッションが表示されます。

## workspace の使い方を確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには主に次の領域があります。

- **New** — Copilot との新しいチャット セッションを開始します。
- **Pull requests** — アプリの GitHub 統合でプルリクエストを表示します。アプリを離れずに、一覧表示や絞り込み、CI 状態の確認、レビューができます。
- **Issues** — GitHub issue を表示し、一覧の絞り込みや issue からのセッション開始ができます。
- **Automations** — スケジュールまたはオンデマンドで実行する保存済みエージェント タスクです。ToDo リストの管理、定期的な保守、繰り返し作業の委任に使えます。まとめでは次のステップとして紹介しますが、別の演習ではありません。
- **Customize** — MCP server、プラグイン、スキルなどを追加します。ここでは Playwright MCP を構成するときに使います。
- **Chats** — ブランチや workspace を作成せずに質問やブレインストーミングを行う簡易チャットです。最初のチャットを開始すると表示されます。表示されない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。
- **Sessions** — リポジトリ フォルダーと、その中のエージェント セッションをグループ化します。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリ ビューが開き、その下のセッションを選ぶと会話に戻ります。セッションごとに独立した workspace があるため、変更が競合することなく複数を同時に実行できます。最初のセッションは星評価を追加するときに開始します。

ワークショップを進めながら、workspace を確認していきます。

> [!TIP]
> 操作方法がわからない場合や、何かができるか知りたい場合は、Copilot に質問してください。手順を案内してくれます。

### 用意されたバックログを確認する

Tailspin Toys のバックログを確認します。これはテンプレートからリポジトリを作成したときに用意されています。

1. サイドバーの **Sessions** で、Tailspin Toys のリポジトリ フォルダーを選択します。配下の個別セッションではなく、リポジトリ フォルダーを選びます。リポジトリの上部ナビゲーションで **Issues** を選択します。
2. issue 番号を決めつけず、次のタイトルを探します。
   - `Allow users to filter games by category and publisher`
   - `Update our repository coding standards`
3. issue を選択して詳細を読みます。各 issue からエージェント セッションを開始できます。最初の小さな変更を完了した後、フィルタリング issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。別のリポジトリの作業項目を確認するには、そのリポジトリをアプリに追加します。

## クイックチャットを試す

アプリに慣れるために、アプリ自体について質問してみます。**quick chat** では、ブランチや worktree を作らずに質問やアイデアの検討ができるため、セッションを必要としない短い質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選択して新しいチャットを開きます。**Chats** がまだ表示されない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。その後 **Chats** が表示されます。
2. アプリのセッションの仕組みについて質問します。

   ```plaintext
   GitHub Copilot app では worktree をどのように使いますか。
   ```

3. 会話ビューで回答を読みます。各セッションが独立した git worktree で実行されるため、変更を競合させずに複数のエージェントを並行して動かせることを確認します。会話は続けることも、新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールしてプロジェクトを接続し、workspace の使い方を確認しました。このレッスンでは、次のことを行いました。

- アプリをインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- workspace の使い方と、**Issues** にあるバックログを確認しました。
- クイックチャットで、その場限りの質問をしました。

次は、[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の概要][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
