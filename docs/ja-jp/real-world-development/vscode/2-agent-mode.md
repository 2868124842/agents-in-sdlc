---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数ファイルの変更やテスト実行などが必要になる場合があります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、デバッグして、変更漏れのファイルを見つけ、もう一度更新するという作業を繰り返します。

ここで役立つのが Copilot エージェント モードです。

Copilot エージェント モードは、IDE 内でより自律的に動作します。開発者と同じように、まずプロジェクトの構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途に向けて設計されています。エージェント モードは IDE 内で作業し、すばやくやり取りしながら進めます。Cloud agent はチームのメンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェント モードがプロジェクトを調査し、関連ファイルを見つけて、複数の変更を連携して行う方法。
- GitHub Copilot エージェント モードで UI とデータレイヤーにまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたので、ユーザーがカテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加したので、残りのデータレイヤー、UI、テストの作業を Copilot エージェント モードで完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリはビルド時にページを静的 HTML として生成します。ページは Drizzle ORM を使い、frontmatter からローカルの SQLite データベースに直接クエリを送信します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、対話機能には標準の DOM API を使う小さく限定的な Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを簡単に起動できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を行います。

1. Codespace に戻ります。前の演習の後なので、`main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。Codespace でホストされている Web サーバーへのプライベート トンネルです。

### Web サイトを確認する

サイトが起動したら機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームの一覧とタイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログも自動で作成されます。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、自律的に計画を立てて複数ファイルを変更し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。このワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順でも同じモデルを指定しています。提案をできるだけ一貫させて、作業しやすくするためです。ただし、LLM は確率的に動作するため、表示される提案が手順と異なる場合があります。これは正常で想定される動作です。

> [!NOTE]
> LLM は確率的に動作するため、Copilot は別の MCP コマンドを使うことがありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーにとって役立つかどうかに基づいて、項目を優先順位付けしています。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot に作業のコンテキストを渡すための指示ファイルを確認します。Web サイトに機能を追加する方法が記載された [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容があります。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - ファイルの種類ごとの指示ファイルへのリンク。Astro、Tailwind CSS、Drizzle などがあります。

> [!TIP]
> 指示ファイルからほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用したり、複雑な内容を管理しやすい小さな単位に分けたり、例やテンプレートを示したりできます。

## フィルタリング機能を実装する

フィルタリングを完了するには、アプリケーションに少なくとも 3 つの変更が必要です。

- データレイヤー (`src/lib/`) にフィルタリング ロジックを追加または調整する。
- フィルタリングの動作をテストするテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

また、コードベースにマージする前にテストを実行して成功することを確認する必要があります。Copilot エージェント モードにこれらの作業を任せられます。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを特定して作業を始めます。データレイヤー ヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調査する様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テスト実行などを行います。タスクやコードベースの複雑さによって、数分かかる場合があります。エディターに **Keep** と **Undo** が表示されることがあります。Copilot が作業を終えると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![テストの実行を確認する Copilot Chat ペインのダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェント モードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. すべてが動作し、コードのレビューも完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

Tailspin Toys Web サイトに機能を追加するため、GitHub Copilot エージェント モードの使い方を学びました。

- GitHub Copilot エージェント モードを使って、UI とデータレイヤーにまたがる新機能を実装しました。
- Copilot エージェント モードでプロジェクトを調査し、関連ファイルを見つけ、複数の変更を連携させました。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューしました。

次は[Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### ボーナス演習 — ページネーションを実装する

ゲームの一覧が増えるにつれ、ページネーションが必要になる場合があります。この演習で学んだことを使って、Copilot にページネーションの実装を依頼します。次の点を考慮してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかなど、ページネーションの動作を検討します。
- 希望する実装に必要なガイダンスをプロンプトに記載します。
- Copilot とやり取りし、変更を依頼してコンテキストを追加する必要がある場合があります。これは Copilot と作業する際の通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 各モードの機能と使いどころ][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
