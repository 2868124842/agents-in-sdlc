---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。Copilot に具体的なコンテキストを共有する方法はいくつかあります。主な方法の 1 つである[指示ファイル][instruction-files]を使って、コード生成に関するガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように適用されるかを確認します。
- 現在の指示を適用して、フィルタリング用の最初のデータ スライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトをもう一度実行し、生成されるコードに新しい標準が反映される様子を確認します。
- 次の演習で Copilot が更新されたガイダンスを使えるように、指示の更新とフィルタリングの一部を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが、設定した標準の一部に従わない場合があります。Copilot の結果は確率的に変化します。この演習の目的は、指示を更新した後の**傾向**の違いを確認することです。出力が一字一句同じになることを期待しないでください。

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
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には、**GitHub Actions Workflows** の詳細なガイダンスもあります。ここにある情報は、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、リポジトリのルートから見た、指示を適用するファイルを指定する glob を設定します。ここでは、`**/*.test.ts` に一致する TypeScript テスト ファイルなどが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` など、ほかの指示ファイルやプロジェクト内の既存ファイルへのリンクがあります。指示を再利用できる小さなファイルに分けて、Copilot がコード生成時に例を参照できるようにします (ここのパスは、リポジトリのルートではなく指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に*Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを更新します。ヘルパー ファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーは、型付きの関数として `db` クライアントを最初の引数に受け取り、型付きの発行元配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加することがあります。その場合も問題ありません。指示を更新した後に動作が一貫することがポイントです。

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準にルールを追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。標準は記載されていますが、コード内のドキュメントに関するルールがまだないため、生成されたヘルパーにはドキュメント コメントがありません。
3. 既存の標準のすぐ下に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` には全体的なガイダンスを、`*.instructions.md` には言語、ファイルの種類、タスクに固有の指示を記載できます。今回追加したドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. Copilot が適切なファイルに注目するよう、`src/lib/publishers.ts` をもう一度選択します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォーム用の発行元データ アクセス ヘルパー。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての発行元の ID と名前を一覧で返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns 発行元オブジェクトの配列を解決する Promise。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を案内し、次の演習で使う実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュできたので、プルリクエストを作成し、バックログにあるドキュメント標準の Issue に関連付けます。手動で操作することもできますが、このプロジェクトに接続済みの GitHub ツールを使って Copilot に依頼します。Issue を見つけ、Issue をクローズする PR を作成して、マージするようプロンプトで伝えます。

> [!NOTE]
> この演習では初めて Copilot に GitHub で操作させます。プロジェクト テンプレートには GitHub のツールがあらかじめ接続されています。セットアップは必要ありません。Copilot が初めてツールを使うときに VS Code から GitHub へのサインインを求められた場合は、画面の手順に従って許可してください。後の演習で、この接続方法 (Model Context Protocol) を学びます。

1. Codespace 内の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルを更新する Issue を探し、現在のブランチから指示の更新と発行元ヘルパーの基盤の両方を説明する PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が GitHub の操作を許可するよう求めたら、コマンドを確認して、問題なければ **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージとブランチの切り替えを依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub の操作とシェル コマンドの実行を許可するよう求めたら、コマンドを確認して、問題なければ **Allow** を選択します。

GitHub Copilot の支援を受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で利用できるように、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は[エージェント モード][next-lesson]で、コードベース全体に機能を追加し、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人向けカスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
