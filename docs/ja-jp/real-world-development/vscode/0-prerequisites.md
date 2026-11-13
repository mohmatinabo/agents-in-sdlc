---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、準備を整えましょう。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動します。また、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ用リポジトリをセットアップする

作成するコードのためにリポジトリのコピーを用意します。[テンプレート][template-repository]から新しいリポジトリを作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれ、演習を進める間に使用できます。

1. 新しいブラウザーウィンドウで、[Tailspin Toys テンプレート][tailspin-template] を開きます。
2. ラボリポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、リポジトリのコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。この後の演習で使用します。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue に取り組むため、自分で issue を登録する必要はありません。

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

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat の agent mode を使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使っている場合は、通常すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension] できます。または、Visual Studio Code で Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択してください。

拡張機能をインストールした後、GitHub アカウントで認証して有効化する必要がある場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力して <kbd>Enter</kbd> を押します。Copilot Chat が有効になります。
4. まだ認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の指示に従って認証してください。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成しました。また、コードを書き始めるときに使う Codespace の作成を開始しました。

## 次のステップ

Copilot に作業を依頼しましょう。[演習 1 - Custom instruction][next-lesson] に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
