---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドへの任意の委任に使えるスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI にも操作に使えるスラッシュ コマンドが多数あります。これらのコマンドを使うと、高度な機能、内部情報、追加設定を利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って Copilot の会話コンテキスト管理を確認します。
- `/model` を使って利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、クラウド エージェントへの委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローは完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、ほかの機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドでコンテキストを確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI がまだ開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分割されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比べます。会話がすでに短い場合は、大きく変化しないことがあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮するタイミングを自分で決めるには `/compact` を使います。無関係な別のタスクを始める場合は、`/clear` または `/new` を使って新しい会話にします。

## モデルを選ぶ

モデルによって得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、使用できるモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学びを共有すると、AI ツールの使い方をお互いに改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンでの `/share` オプションを確認します。
2. セッションを共有する場合は、目的に合った方法を選びます。たとえば `/share file` は、ローカルに Markdown ファイルとしてエクスポートします。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容を公開しないでください。

## オプション: 作業を並行または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに役立つコマンドもあります。

- `/fleet` を使うと、独立したサブタスクをサブエージェントに割り当て、並行して実行できます。
- `/worktree` を使うと、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` を使うと、非同期で作業し、プルリクエストを開くこともある Copilot cloud agent にタスクを送れます。

これらのコマンドは、新しい worktree やリモート作業を開始する場合があるため、任意の内容です。試す前に、範囲を明確にした新しいタスクを用意し、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶには、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定の変更、セッションの共有、内部の動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って Copilot の会話コンテキスト管理を確認しました。
- `/model` を使って利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、クラウド エージェントへの任意の委任に使えるコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。ここまでに学んだ内容と、さらに学習を続ける方法を[振り返って][next-lesson]、ワークショップを締めくくりましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
