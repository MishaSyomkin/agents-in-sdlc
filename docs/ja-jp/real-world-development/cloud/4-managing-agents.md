---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。Mission control から、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加のガイダンスを送ったりできます。

このレッスンでは、次のことを行います。

- agents ページを使って cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードを依頼した後、チームはライト モードも追加するとよいと考えました。サイトのスタイルとモード切り替え機能を更新しているため、一緒に実装するのが自然です。エージェントにライト モードとハイ コントラスト モードの両方を実装するよう、作業の方向を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方に表示されるタスク一覧を確認します。たとえば次のタスクが表示されます。
   - コードのドキュメントを更新する
   - ゲーム変更用の API を生成する
   - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が行った作業を確認します。次の作業が表示される場合があります。
   - リポジトリからコードをチェックアウトする
   - Copilot が作業する環境を作成する
   - MCP server を設定する
   - 割り当てられたタスクを完了するための各種手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインも確認します。PR と変更されたファイルを表示し、追加で監視できます。

## cloud agent の作業を調整する

進行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードも含めるよう依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。Copilot が使ったタイトルや作業状況によって表示名は異なります。

   ![Mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、作業が始まったことを示すメッセージが表示されるまで数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを追加します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents ページで、作業中の cloud agent に指示を追加するダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に取り込んだことを確認します。

## Copilot の作業を待つ

これまでと同様に、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で進めていた手順が終わると、新しい依頼が作業に加わります。

作業には数分かかります。その間に、これまでに学んだことを振り返ってもよいでしょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視し、案内する中心的な場所である Copilot agents ページを確認しました。Mission control では、進行中と完了済みのタスクを追跡し、作業をレビューし、進行中のタスクの範囲やガイダンスを変更できます。

このレッスンでは、次のことを行いました。

- Mission control と agents ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を完成させている間に、次は[作成されたプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
