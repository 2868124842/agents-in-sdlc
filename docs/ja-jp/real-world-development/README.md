---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を段階的に計画、実装し、動作をテストし、プルリクエストをレビューして、エージェントと作業を管理します。

## シナリオ

あなたは、ボードゲームのクラウドファンディングを手掛ける架空の会社 Tailspin Toys に新しく加わった開発者です。チームのバックログは GitHub Issue として登録済みなので、すぐに取り組めます。機能追加にはフィルタリングやページネーションがあり、品質向上にはアクセシビリティやコーディング標準への対応があります。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。開発スタイルに合った環境を選んでください。各ワークショップは個別にセットアップし、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実践的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進捗を確認して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントをエディターから利用できます。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
