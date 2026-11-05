---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードへのドキュメント追加、デザイン チームが試す関連ゲーム機能の実装、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームでのレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監視なしで作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権は作業に使うブランチに**のみ**あります。
- Cloud agent は GitHub Actions 内で動作し、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP server を含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は MCP server などの外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあります。その間に休憩するか、これまでに学んだことを振り返ってください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc の doc comment など、更新されたコードとドキュメントを確認します。変更内容は状況によって異なります。

   確認時には、演習 2 の最初に見たカスタム命令に由来する TSDoc の doc comment、TypeScript の規約、コメント ヘッダーを探します。

6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに追加されたことを確認します。すべて問題なければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りすることもできます。チーム メンバーに依頼するのと同じように、PR またはコードのインライン コメントで Copilot にメンションすると、コメントを受けて新しいセッションが始まり、変更に対応します。結果は確率的に変わるため、特定のプロンプトを指定することはできません。依頼できる変更の例は次のとおりです。

- 各コード ファイルの先頭に、ファイルの役割を短く説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc の doc comment を追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例、または追加したいドキュメントについての別の提案を使って、**@copilot** をメンションしたコメントを投稿し、生成されたドキュメントの変更を依頼します。
2. **View Session** を選択し、Copilot が変更を行う様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、PR に新しい commit が追加されたことを確認します。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復して改善してください。問題がなければ、PR をドラフトからレビュー可能な状態にして、`main` ブランチにマージできます。

![PR をレビュー可能な状態にする画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブで生成されたコードをレビューします。
5. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに追加されたことを確認します。すべて問題なければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot cloud agent に追加の変更を依頼できます。たとえば表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc の doc comment を追加したりできます。この issue はカスタム命令を更新する**前**に割り当てたため、その点も確認してください。**Conversation** タブで新しいコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で Mission control から追加を依頼したライト モードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存する local storage の利用
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに追加されたことを確認します。すべて問題なければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチに切り替えて、アクセシビリティ機能を手動でテストできます。ターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に入れて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用カスタム エージェントが、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot を導いたことを確認します。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、バックログの実際の issue を渡して別の作業に集中するときに役立ちます。習慣づけるために、リポジトリでいくつかの issue を作成して Copilot に割り当ててみてください。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム命令ファイルを確認し、cloud agent の出力に与える影響を確認しました。**
- **issue を Copilot cloud agent に割り当て、非同期に環境を設定して計画し、実行する様子を確認しました。**
- **アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。**
- **Copilot agents ページを Mission control として使い、アクセシビリティのセッションを監視して途中で方向を調整しました。**
- **Copilot cloud agent のプルリクエストをレビューして反復し、`@copilot` に変更を依頼して、ワークフローを承認しました。**

## レビューと次のステップ

Cloud agent harness を完了しました。引き続き試す場合は、ほかの harness で関連する機能を確認できます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI harness](../../cli/)** — Plan と Autopilot モード、agent skill、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの 2 つの変更をリリースします。

自分のリポジトリで、次の案を試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てます。
- 別の分野を担当するカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
