---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように作業してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように適用されるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、ここでは指示ファイルの変更前後を比べるプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで影響を比較することが難しいためです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスのガイドラインと要件があります。

- データレイヤーには常に単体テストが必要です。
- UI はダーク モードにし、現代的なデザインにします。
- コードのドキュメントには TSDoc のドキュメント コメントを使います。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと設定を Copilot に渡せます。関連性の高い提案やコード スニペットが得られるように、好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルやタスクだけに適用する指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、作成中のものの概要、ベスト プラクティスなど、Copilot に送るほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に使用できます。TypeScript などの言語、UI コンポーネントや単体テストの作成などのタスクに関する指示を定義できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` frontmatter がある `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[カスタム指示の対応形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく扱うことは、このワークショップの範囲外です。ただし、サンプル プロジェクトに含まれる例から一般的な方法を確認できます。

- `copilot-instructions.md` には、作成中のものやプロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データレイヤーなど、ファイルの種類やタスクに関する具体的な指示を記載します。
- 自然な言葉で明確に説明し、コードのよい例と避けるべき例を示します。

指示ファイルの作成方法は 1 つではなく、AI の使い方と同様に、プロジェクトやシナリオに合った方法を試して見つけます。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意すると役立ちます。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクの指示ファイルがあります。
>
> Copilot に指示ファイルの生成を依頼することもできます。各環境での操作は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init`)。該当する場合は、各環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

少し時間を取って、このリポジトリに含まれる指示ファイルを読みます。コアの `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。ここにある情報は、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示を適用するファイルを示す glob を設定します。ここでは、`**/*.test.ts` に一致する TypeScript テスト ファイルなどが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` など、ほかの指示ファイルやプロジェクト内の既存ファイルへのリンクがあります。指示を再利用できる小さなファイルに分けて、Copilot が例を参照できるようにします。ここにあるパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 新しいリポジトリ標準を追加する

ここで編集するのは、ドキュメントを TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーに記述するというプロジェクト全体のルールです。後の演習で Issue を割り当てるときに、cloud agent がこのルールを使います。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、変更をきちんとコミットするため、ここではブランチを作成します。

1. Codespace のターミナルから、ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の手順で確認したように、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するため、コーディング標準にコメントとファイル ヘッダーのルールを追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準はありますが、コード内のドキュメントに関するルールがまだないため、生成されたヘルパーにはドキュメント コメントがありません。
3. 既存の標準のすぐ下に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` には全体的な標準を、`*.instructions.md` には言語、ファイルの種類、タスクに固有の指示を記載できます。ドキュメント コメントのルールなど、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。cloud agent が指示を使用できるように、変更を `main` にマージします。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue に割り当てるとき、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業しても構いません。その場合は、ブランチを作成する手順を省略し、`main` に直接コミットできます。ここでブランチを使うのは、実際のプロジェクトで行う方法に近づけるためです。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。後のハーネスで cloud agent が作成するプルリクエストに、この標準が適用されていることを確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
