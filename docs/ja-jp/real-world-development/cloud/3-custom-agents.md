---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発フローの特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、Copilot に具体的な命令、ベスト プラクティス、コーディング パターン、分野固有の知識を伝えます。チームの専門知識を、アクセシビリティ、セキュリティ、テストなどの再利用可能なエージェントとしてまとめられます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェントや、安全なコーディング プラクティスに従うセキュリティ エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門性、命令を示す Markdown プロンプトを記述します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept]には、概念上の共通点があります。どちらも主に Markdown ファイルで定義し、AI の作業方法を指示します。分かりやすく言うと、**カスタム エージェントは作業者、skill はツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部として skill（ほかのエージェントも含む）を調整するよう設計されています。このラボでは、アクセシビリティ用のカスタム エージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストのワークフローやテスト実行を管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使い、次のことを確認します。

- カスタム エージェントの定義方法
- カスタム エージェントへのタスク割り当て

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近、テキストと背景色のコントラストが不十分なため、ダーク テーマが読みにくいというフィードバックが寄せられました。このアクセシビリティの問題に対応するため、デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。ここではカスタム エージェントを使って機能を生成します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Copilot を導く内容を理解するため、ファイルを確認します。

1. Codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイト用コードを生成する際の主な責務
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## issue を作成して割り当てる

Mission control は、この環境のエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業の方向を調整できます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選び、割り当てのダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面で、カスタム エージェントの Accessibility が選択されている様子](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取り組み始めます。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。特定のタスクや分野に合わせた専門的な AI アシスタントを使い、チームの知識や標準を再利用可能な形にまとめて、Copilot の作業を導けます。

このレッスンでは、次のことを確認しました。

- カスタム エージェントの定義方法
- カスタム エージェントへのタスクの割り当て

Copilot がハイ コントラスト モードを実装している間に、次は Mission control から[エージェント セッションを監視して方向を調整します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
