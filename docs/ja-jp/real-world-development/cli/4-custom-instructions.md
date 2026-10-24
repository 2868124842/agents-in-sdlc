---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、文書化の標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、どのようなコードを作成するかだけでなく、どのように構成するかも伝えられます。フィルタリング機能を実装したので、Copilot が使った指示を確認し、文書化の標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別の指示ファイルが、エージェントにどのように適用されるかを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。

- コメントには、コードの繰り返しではなく、意図や自明でない判断を記載します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を記述した TSDoc/JSDoc を追加します。該当する場合は、注入される `db` 引数も記載します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連コードが変わったらコメントも最新の状態にします。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業するための情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと設定を Copilot に渡せます。これにより、より適切な提案やコード スニペットが得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルやタスクだけに適用する指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、プロジェクトの概要、全体的なガイダンスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイダンスを提供します。

> [!NOTE]
> ほかの指示形式のサポート状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

作業を始めやすいよう、スターター プロジェクトには指示ファイルが含まれています。変更を加える前に内容を確認し、影響を比べましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。これらの指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示が適用されるファイルを指定する glob を設定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは有用ですが、まだ不足があります。コアの `copilot-instructions.md` を変更して、新しい TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。およそ 35 行目にあります。
2. 最後の箇条書きとして次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルタリングの会話を終了して再開します。これにより、機能のコンテキストを維持しながら、新しいガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がこのプロジェクトの指示をどのように参照するかを確認し、新しい標準を機能に適用しました。次のことを行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行][next-lesson]し、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
