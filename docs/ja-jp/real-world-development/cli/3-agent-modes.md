---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で issue からフィルタリング機能を実装して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って、必要なものを適切な方法で作れるよう支援します。このレッスンから、issue を起点に機能を実装し、コードの妥当性と機能の動作を確かめ、最終的にプロジェクトへマージする一連の開発プロセスを進めます。

> [!NOTE]
> この機能開発では、同じ会話とブランチを使い続けます。通常はファイル種別ごとに異なるブランチや PR を使いますが、ここでは中心的な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の新しい会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送したブラウザーで機能を手動検証します。

この機能開発を続ける中で、リポジトリの命令を更新し、既存の `quality-checks` スキルを調整し、MCP による検証を追加し、QA エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリやパブリッシャーで絞り込めるようにする必要があります。バックログの issue には機能が記されていますが、カテゴリを組み合わせる方法など、実装前に合意したい細部もあります。Plan モードでその方針を決めてから、範囲を限定して Autopilot に実装を任せます。

## 背景

AI コーディング エージェントを開発フローに加えても、基本は変わりません。むしろ、基本を守ることが一層重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記した issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、ほとんどの開発フローはこの流れを基本にしています。

この標準的な進め方に沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: 開発者とエージェントが協力します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から開始する

フィルタリング機能に着手するため、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を開始します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** という issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7`（または同様の番号）で始まることを確認します。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、Copilot に作業を依頼する前に機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 先ほど追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、実装方法を検討し、計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に沿って回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上下にスクロールして確認します。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を書き、内容を確認したので、実装に進みます。Autopilot を使うと、Copilot が完了したと判断するまで問題に取り組めます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様のラベルの選択肢を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを作成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。計画を先に確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前に問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を実行し、フィルタリングの実装とテストを確認します。
3. 結果を issue と計画で決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードを確認したら、実際に動くか確かめます。前の手順と同じようにアプリを起動し、Codespaces の転送ポートを通じてサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を利用可能と表示したら、**Open in Browser** を選択します。
3. カテゴリ、パブリッシャー、計画で決めた組み合わせを試します。
4. リセット時と結果が空の場合の動作が、issue と合意内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を実装し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の新しい会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送したブラウザーで機能を手動検証しました。

次は、コードが文書化されたプラクティスに従うよう、[カスタム命令を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
