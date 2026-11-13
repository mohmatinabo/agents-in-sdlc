---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、準備を整えましょう。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。Codespace では instruction file を編集し、cloud agent が作成した成果をレビューします。

## ラボ用リポジトリをセットアップする

作成するコードのためにリポジトリのコピーを用意します。[テンプレート][template-repository]から新しいリポジトリを作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれ、演習を進める間に使用できます。

1. 新しいブラウザーウィンドウで、[Tailspin Toys テンプレート][tailspin-template] を開きます。
2. ラボリポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、リポジトリのコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。この後の演習で使用します。

## Codespace を作成する

次に、Codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザー上で直接コードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えた IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもずっと早く準備できます。その間に GitHub Copilot のほかの機能を確認しておきましょう。次の演習で詳しく扱います。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。しばらくは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できるように構成されています。どちらの環境にも、演習に必要なものがあらかじめインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択してください。VS Code が Codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成しました。また、Copilot cloud agent と一緒に作業するときに使う Codespace の作成も開始しました。

## 次のステップ

cloud agent が従う custom instruction を追加しましょう。[演習 1 - Custom instruction][next-lesson] に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
