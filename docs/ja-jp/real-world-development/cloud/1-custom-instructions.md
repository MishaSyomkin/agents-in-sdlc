---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[命令ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように進めるかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス単位の命令ファイルが、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の harness とは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を使って非同期に作業するため、リアルタイムで並べて効果を示すのが難しいためです。後の演習で cloud agent が作成したプルリクエストを確認するときに、命令ファイルの影響を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に渡される単一の命令ファイルです。技術スタック、開発中のプロジェクトの概要、ベスト プラクティスなど、ほとんどの Copilot Chat や CLI リクエストに関係するプロジェクト全体の情報を記載します。
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

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` にプロジェクト全体のコーディング標準を追加します。cloud agent は、後の演習で割り当てる issue に取り組むときに、この標準を読み取ります。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Cloud agent は issue の対象ブランチにある命令を読み取ります。後の演習では `main` を対象に issue を割り当てるため、変更も `main` にマージする必要があります。

2. `.github/copilot-instructions.md` をもう一度開きます。
3. **Code formatting requirements** セクションを探します。27 行目付近にあります。既存のコーディング標準には、コード内ドキュメントの規則がまだありません。
4. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc の doc comment を指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のセクションで確認したように、命令ファイルは、グローバルなガイダンスにはリポジトリ レベル（`.github/copilot-instructions.md`）を、特定の言語、ファイル種別、タスクには `*.instructions.md` を使えます。今回追加した doc comment の規則のような、プロジェクト全体の標準はリポジトリ レベルのファイルに記載します。

## 命令の更新を commit、push、マージする

次の演習で issue を Copilot に割り当てると、`main` からブランチが作成されます。cloud agent が命令を参照できるよう、変更を `main` にマージします。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。次の演習で cloud agent が作業するとき、この命令を読み取れるようになります。

> [!TIP]
> このワークショップではブランチを使い、実際のプロジェクトに近い手順を体験します。`main` で直接作業する場合は、ブランチを作成せずに `main` に commit してもかまいません。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りの harness で cloud agent が作成するプルリクエストを確認するときに、この標準が使われます。

次は、Copilot cloud agent に[最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令作成のベスト プラクティス][instructions-best-practices]
- [より良い Copilot カスタム命令を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
