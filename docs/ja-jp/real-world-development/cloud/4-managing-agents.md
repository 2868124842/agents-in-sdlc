---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent にユーザー エクスペリエンスの改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

Copilot cloud agent に割り当てたタスクを管理するツールはいくつかあります。たとえば、GitHub.com の[エージェント ページ][agents-page]です。このミッション コントロールでは、開いているプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。エージェントが行った操作を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次のことを行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼し、作業方針を調整します。

## シナリオ

ハイ コントラスト モードを作成するようエージェントに依頼した後、チームはライト モードも追加するよい機会だと気付きました。サイトのスタイルと切り替え機能を更新しているため、ハイ コントラストに加えてライト モードも実装するようエージェントに依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインにタスクの一覧が表示されます。次のような割り当て済みタスクを確認します。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択します。Copilot が実行した操作を確認します。たとえば、次のような内容があります。
   - リポジトリのチェックアウト。
   - Copilot が作業する環境の作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクを完了するためのさまざまな手順。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される PR ペインも確認します。PR と変更ファイルを表示して、さらに監視できます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認できたので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの作成に関連するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![ミッション コントロールでアクセシビリティ セッションを表示](../../../_images/ex6-accessibility-session.png)

2. Copilot のセットアップが終わり、作業が始まるまで数分間セッションを確認します。次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Agents ページで Copilot の作業中セッションに指示を追加するダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業計画に追加する様子を確認します。

## Copilot に作業を任せる

Copilot は、更新されたタスクに取り組みます。メッセージを送信した時点で進行していた手順が完了した後、追加の依頼を作業に反映します。

ここでも数分かかるため、少し休憩して、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って cloud agent のタスクを監視し、案内する方法を学びました。このミッション コントロールでは、進行中と完了済みのタスクを追跡し、実行中の作業をレビューして、タスクの範囲を変更したり追加のガイダンスを送ったりできます。

次のことを行いました。

- ミッション コントロールとエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、Copilot が作成したプルリクエストを[改善する方法][next-lesson]を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
