---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織はほとんどありません。未解決のセキュリティ課題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどが考えられます。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作が安全に実行されるようにします。
- Issue の範囲を明確にする重要性を確認します。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には、解消したい技術的負債があります。最初のサイトを開発した委託先はドキュメントを適切に残しておらず、確認すると、ほとんど何もありません。まずは、すべてのエクスポートされた関数に TSDoc のドキュメント コメントを追加したいと考えています。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要ありません。UX の受け入れテストに使えるものを求めています。これは現在の作業を止める要因ですが、優先度の高い別の Issue もあります。

どちらも優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てると、開発者がほかの作業を進めている間に非同期で実装できます。後から Copilot の変更をレビューして、期待どおりか確認できます。

## GitHub Copilot cloud agent を使う

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、複数の方法があります。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備して、タスクに取りかかります。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳細を確認すると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に進めながら、学び、試して、調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を学びました。カスタム指示の演習をすでに完了している場合は、`.github/copilot-instructions.md` が役立つことを確認済みです。まだの場合は、今のうちに指示ファイルを簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、リポジトリにある指示ファイルを読み取り専用で確認すると、後でその効果を確認できます。

リポジトリの github.com ページ、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に、TypeScript の規約と TSDoc コメントに関する要件に注目します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この指示が `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、生成されたプルリクエストで TSDoc コメント、TypeScript の規約、コメント ヘッダーがどのように適用されるかを確認します。後の演習で PR をレビューするときにも、この点を確認します。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コードの作成には通常、特定の開発環境とセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェア エンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` ファイルで設定した[特別なセットアップ ワークフロー][setup-workflow]を作成して、作業開始前に必要なツールや依存関係を使えるようにできます。ワークショップの流れをスムーズにするため、この設定は事前に用意されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *your steps* に必要な最小限の権限を設定します。Copilot には操作用の専用トークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部でリポジトリをクローンする場合は `contents: read` 権限が必要です。手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションしてシードし、ビルドとテストでデータを利用できるようにします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub Actions ワークフローと似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot が PR で作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを指定すると、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、ワークフローが正常に動作することを確認できます。

## ドキュメントを追加する

ドキュメントが重要であることは誰もが理解していますが、多くのプロジェクトでは情報が古かったり、まったくなかったりします。このような技術的負債は放置されやすく、生産性が下がり、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメントの作成が得意なため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成させ、後の演習で変更を確認します。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue の作成画面を開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成していることを確認します。

> [!NOTE]
> Copilot が To Do リストを作成するまでに数分かかる場合があります。Copilot は環境を準備し (先ほど確認したワークフローを実行し)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新後のインジケーターを確認するために、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot の作業を確認します。コードと状態の調査、方針を検討するための思考の一時停止、コードの作成などが含まれます。

完了するまで数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、非同期で作業し、開発者が別の作業に集中できるようにすることです。この機能を活用し、もう 1 つのタスクを cloud agent に割り当ててから、アプリへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チームのために関連ゲームのセクションを追加する作業は時間がかからないかもしれませんが、その時間をほかのタスクに使うこともできます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue の作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当て画面を開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

間もなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように動作するため、常に監視する必要はありません。作業が完了したらレビューできます。ここでは、カスタム エージェントの作成と利用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使い、Issue を割り当てて非同期で処理できることを学びました。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などを Copilot に任せられます。

次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作が安全に実行されるようにしました。
- Issue の範囲を明確にする重要性を確認しました。
- Issue を Copilot に割り当てました。

Cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントの作成と利用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
