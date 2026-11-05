---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Pages のワークフローを構成し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Space Quiz を GitHub Pages でデプロイして、誰でもアクセスできるようにします。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイを構成するよう依頼します。
- マージする前に、ワークフローとプルリクエストを確認します。
- デプロイを追跡し、公開されたサイトを確認します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、試すには手軽な方法が必要です。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに公開 URL からアクセスできます。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプルリクエストがマージされ、GitHub Actions の Deploy website ワークフローが成功し、yourname.github.io/space-quiz/ でサイトが公開されるまでの 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローを確認してデプロイを検証する

デプロイが成功したことを確認するため、ワークフローと公開サイトを順に確認します。

1. リポジトリに README がすでにある場合、それが変更されていないことを確認します。
2. プルリクエストの変更を確認します。ワークフローのビルド手順とデプロイ手順も確認してください。
3. 必須のプルリクエスト チェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行を追跡し、成功するまで待ちます。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを行えない場合は、提示された手順に従います。デプロイが成功して公開サイトが読み込まれるまでは、作業を完了と見なさないでください。

## まとめと次のステップ

Space Quiz をライブ サイトとして公開し、エージェントの報告だけに頼らずデプロイを確認しました。次は、[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
