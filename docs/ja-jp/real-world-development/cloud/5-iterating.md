---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot と一緒に、ユーザー エクスペリエンスの改善と機能追加に関するいくつかのタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが改善できる関連ゲーム機能を作成し、ハイ コントラストとライト モードの切り替えなどアクセシビリティ機能を実装するよう Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまで何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームのほかのメンバーがレビューできるようにする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は、監視されずに非同期でタスクを実行するため、安全を保つためのセキュリティ制限があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスはコードの作成に使うブランチだけに制限されます。
- Cloud agent は GitHub Actions 内で実行され、作業用に分離された一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR で、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定内**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR には **[WIP]** の表示があります。作業が完了するまで待ちます。数分かかることがあります。その間に、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TSDoc のドキュメント コメントなどが含まれます。具体的な変更内容は異なる場合があります。

    変更を確認するときは、演習 2 の最初に確認したカスタム指示ファイルに基づいて、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーが追加されているかを確認します。

6. 変更を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方向ではありません。チーム メンバーと同じように、PR のコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、依頼する内容を正確に指定することはできません。Copilot に変更を依頼する例を次に示します。

- 各コード ファイルの先頭に、役割を簡単に説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例か、コードベースに必要なドキュメントの変更を選び、**@copilot** をメンションして生成されたドキュメントの変更を依頼するコメントを追加します。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が新しいセッションを開始して変更する様子を確認できます。
3. **Back to pull request** を選択して PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選択して変更を確認します。

満足するまで繰り返し Copilot に改善を依頼できます。問題がなければ、ドラフトからレビュー可能な状態に変更して、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 変更を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションでワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) を `<branch-name>` に指定して、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同様に、コメントを追加すれば Copilot cloud agent に更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc のドキュメント コメントを追加したりできます (このタスクを割り当てたのは、カスタム指示を更新する前でした)。**Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で割り当てたハイ コントラスト モードと、演習 4 でミッション コントロールから追加を依頼したライト モードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR には **[WIP]** の表示があります。Copilot の作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 実装をレビューするときは、次の点に特に注意します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保持するためのローカル ストレージの使用。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 変更を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に指定して、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次に、アプリを起動してブラウザーでハイ コントラストとライト モードの切り替えを試し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントは、アクセシビリティのベスト プラクティスに従って機能を実装するよう Copilot を案内しました。アクセシビリティについて気になる点や改善案があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、実際のバックログ項目を任せて自分が別の作業に取りかかる場合に役立ちます。この方法に慣れるため、リポジトリに Issue を追加し、Copilot に割り当ててみましょう。次のようなアイデアがあります。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで、次のことを行いました。

- このリポジトリにあるカスタム指示ファイルを確認し、cloud agent の出力にどう反映されるかを確認しました。
- Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の作業を確認しました。
- アクセシビリティ向けのカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページをミッション コントロールとして使い、作業中にアクセシビリティ セッションを監視して案内しました。
- Copilot のプルリクエストをレビューして改善し、`@copilot` をメンションして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学習する場合は、ほかのハーネスで内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 機能を使って、レビュー済みの変更を 2 件リリースします。

自分のリポジトリで、次のような取り組みを試してください。

- リファクタリングまたはテスト範囲の Issue を cloud agent に割り当てます。
- 別の分野に対応するカスタム エージェントを作成します。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
