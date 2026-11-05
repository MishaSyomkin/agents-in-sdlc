---
title: "演習 4 - カスタム エージェント"
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

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近、テキストと背景色のコントラストが不十分なため、ダーク テーマが読みにくいというフィードバックが寄せられました。このアクセシビリティの問題に対応するため、デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントで機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Copilot を導く内容を理解するため、ファイルを確認します。

1. Codespace に戻り、アクセシビリティ作業用に `main` から新しいブランチを作成します。フィルタリングの PR は演習 3 のブランチに分けておきます。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

4. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイト用コードを生成する際の主な責務
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義したカスタム エージェントが Copilot Chat ビュー下部の agents dropdown に表示されます。カスタム エージェントを選択すると、そのエージェントの命令とツールに沿ったチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューの agent dropdown で **Agent** が選択されていない場合は選択します。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部の agents dropdown を選択します。現在の agent（既定では **default**）が表示されます。
3. 利用可能な一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。編集はファイル パスと差分とともにチャット内に表示されます。
6. 編集が追加されると、**Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> 実際のリポジトリを変更するため、処理には数分かかる場合があります。Astro コンポーネント、CSS、関連テストなどを編集します。

次の演習では、進行中の作業をレビューし、必要に応じて方向を調整します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。特定のタスクや分野に合わせた専門的な AI アシスタントを使い、チームの知識や標準を再利用可能な形にまとめて、Copilot の作業を導けます。

このレッスンでは、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- Copilot Chat の agent mode でカスタム エージェントを使いました。

次は、[エージェントの作業を監視して調整します][next-lesson]。変更を確認し、同じセッションにライト モードの切り替えも追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
