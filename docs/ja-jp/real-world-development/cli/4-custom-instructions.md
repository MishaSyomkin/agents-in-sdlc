---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認してドキュメント標準を追加し、フィルタリング機能に適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[命令ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も説明できます。フィルタリング機能を作成したので、Copilot が参照した命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパス単位の命令ファイルがエージェントに届く仕組みを確認します。
- コーディング標準が守られるよう、命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントにはコードを言い換えるのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を付けます。該当する場合は、注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変わったらコメントも最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

命令ファイルを使い、これらのプラクティスに沿ってタスクを進めるための情報を Copilot に伝えます。

## 命令ファイル

カスタム命令を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させられます。好みのコーディング規約、ライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する命令と、特定のファイル種別に適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の命令ファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクごとのガイダンスを提供します。

> [!NOTE]
> 環境によって、その他の命令形式への対応状況は異なります。特定の形式を使用する前に、[カスタム命令の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトの命令ファイルを確認する

変更を加える前に、スターター プロジェクトに含まれる命令ファイルを確認し、現在の内容を把握します。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この命令はリポジトリ内での Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存プロジェクト ファイルへのリンクを確認します。これにより、ガイダンスを再利用しやすい単位に分割し、参考例を示せます。

## チームの方針に合わせて命令ファイルを更新する

既存の命令に不足している内容を補うため、中心となる `copilot-instructions.md` を編集し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。更新した命令を読み込ませつつ、フィルタリングのコンテキストを保つため、会話を終了して再開します。

1. フィルタリングの会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードの内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot がこのプロジェクトの命令ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの命令とパス単位の命令ファイルがエージェントに届く仕組みを確認しました。
- 命令ファイルを更新して、コーディング標準を適用しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、繰り返し実行できるチェックを行うため、[quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム命令を追加する][instruction-files]
- [カスタム命令作成のベスト プラクティス][instructions-best-practices]
- [より良い Copilot カスタム命令を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
