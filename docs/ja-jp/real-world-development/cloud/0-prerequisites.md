---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。コードを作成するための Tailspin Toys リポジトリを自分用にコピーし、[Codespace][codespaces] を起動します。演習では、Codespace で指示ファイルを編集し、cloud agent が作成した変更をレビューします。

## ラボ リポジトリをセットアップする

コードを作成するため、[テンプレート リポジトリ][template-repository]から自分用のインスタンスを作成します。新しいインスタンスにはラボに必要なファイルが含まれ、演習を進める際に使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力](../../../_images/ex0-repository-settings.png)

4. 後の手順で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えます。

## Codespace を作成する

次に、Codespace を使ってラボの演習を行います。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーで直接コードの作成、実行、デバッグができます。複数のプログラミング言語、拡張機能、ツールに対応した、機能豊富な IDE が用意されています。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりも早く完了します。その間に GitHub Copilot のほかの機能を確認できます。次の演習で詳しく扱います。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。ここでは、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] 内で実行できます。どちらも、演習に必要な前提条件があらかじめインストールされた環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が作成されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ リポジトリのコピーを作成できました。また、Copilot cloud agent と一緒に演習を進めるための Codespace の作成も開始しました。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレート リポジトリからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
