---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で進める必要があるタスクや、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。中でも重要なのは、コード生成のガイダンスを提供する[命令ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス単位の命令ファイルが、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える仕組みを確認します。
- *現在の*命令を使い、フィルタリングの最初のデータ部分（publishers helper）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認します。
- 命令の更新とフィルタリングの最初の変更を `main` に commit し、次の演習で Copilot が最新のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、命令を更新した後の動作の*傾向*を確認することであり、出力を 1 文字ずつ一致させることではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ず単体テストを用意します。
- UI はダーク モードで、現代的な印象にします。
- TSDoc の doc comment を使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、これらのプラクティスに沿ってタスクを進めるための情報を Copilot に伝えます。

### カスタム命令

カスタム命令を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させられます。コーディング規約、ライブラリ、コメントの形式などを指定できます。命令はリポジトリ全体に対しても、特定のファイル種別に対しても作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に渡される単一の命令ファイルです。技術スタック、開発中のプロジェクトの概要、ベスト プラクティスなど、ほとんどの Copilot Chat や CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別にガイダンスを提供します。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などのタスクに利用できます。

> [!NOTE]
> IDE で作業する場合、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリの命令ファイルと `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準形式の命令ファイル][custom-instructions-support]もサポートしています。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法の詳細はこのワークショップの範囲外ですが、サンプル プロジェクトのファイルは代表的な構成を示しています。基本的な方針は次のとおりです。

- `copilot-instructions.md` には、開発中のもの、プロジェクト構造、全体のコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データ レイヤーなどのファイル種別や、特定のタスクに対する具体的な命令に使います。
- 自然な言葉で、明確なガイダンスを記載します。望ましいコードと望ましくないコードの例を示します。

命令ファイルにも AI の使い方にも、唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルを作成してもらうこともできます。各環境で提供方法は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。該当する場合は、その環境のレッスンで説明します。
>
> テンプレートや出発点を探すには、命令ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれる命令ファイルを読み、Copilot に渡されるプロジェクト情報を確認します。エディターか GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関するガイダンスにも注目します。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトの単体テスト作成に関する命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、命令を再利用しやすい単位に分け、コード生成の参考例を示せます。（記載されているパスは命令ファイルからの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

変更を安全に分けるため、作業用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム命令の効果を見るため、現在の命令を使ってプロンプトを送信します。その後、命令を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習から開いているエディター タブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、helper の保存先を Copilot に示します。
3. Chat ビューの agent dropdown で **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。helper とテストなど、複数のファイルに変更が及ぶ場合があります。
6. 提案された helper が型付き関数で、最初の引数に `db` client を受け取り、型付き publishers 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc の doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくても doc comment を追加することがあります。その場合も問題ありません。この演習では、命令を更新した後に動作の一貫性が高まることを確認します。

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` にプロジェクト全体のコーディング標準を追加し、コード提案を改善します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。既存の標準にはコード内ドキュメントの規則がまだありません。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc の doc comment を指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、グローバルなガイダンスにはリポジトリ レベル（`.github/copilot-instructions.md`）を、特定の言語、ファイル種別、タスクには `*.instructions.md` を使えます。今回追加する doc comment の規則のような、プロジェクト全体の標準はリポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択して、Copilot が対象ファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用し、次の演習で使うコードに反映できました。

## 命令を commit してブランチを push する

命令ファイルもほかのリポジトリ ファイルと同じように、ソース管理で管理します。変更を commit してリポジトリに push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 命令と helper の変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. 命令と helper の基盤に関して Copilot が変更したファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、プルリクエストを作成し、バックログのドキュメント標準に関する issue と関連付けます。GitHub のツールはプロジェクトに接続済みのため、Copilot に issue を探して PR を作成し、マージするよう依頼できます。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは GitHub のツールが workspace に接続済みのため、追加設定は不要です。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。表示される手順に従ってください。この接続の仕組みである Model Context Protocol (MCP) は、後の演習で説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 命令ファイルの更新に関する issue を見つけ、現在のブランチから PR を作成するよう Copilot に依頼します。命令の更新と、今後のフィルタリング作業に使う publishers helper の基盤の両方を説明します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使い、PR をマージしてローカル ブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作や shell command の **Allow** を求めたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の命令でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従う様子を確認しました。
- 次の演習で作業を続けられるよう、命令の更新と helper の基盤を `main` に commit しました。

次は、Copilot がコードベース全体に機能を追加するときに、[agent mode で命令を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令作成のベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
