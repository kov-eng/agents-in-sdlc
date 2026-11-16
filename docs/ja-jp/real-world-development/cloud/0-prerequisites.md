---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot クラウド エージェントの演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces]を起動します。Codespace では指示ファイルを編集し、クラウド エージェントが作成した成果をレビューできます。

## ラボ用リポジトリを設定する

演習で使うコードのコピーを作成するには、[テンプレート][template-repository]から新しいインスタンスを作成します。新しいインスタンスにはラボに必要なファイルがすべて含まれます。演習中はこのリポジトリを使用します。

1. 新しいブラウザー ウィンドウで、[Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のリポジトリを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。ラボの後半で使用します。

## Codespace を作成する

次に、ラボの演習を行う Codespace を作成します。

[GitHub Codespaces][codespaces]はクラウドベースの開発環境です。ブラウザーで直接コードを作成、実行、デバッグできます。複数のプログラミング言語、拡張機能、ツールに対応した高機能な IDE が用意されています。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりもはるかに早く完了します。その間に GitHub Copilot の他の機能を確認しておきましょう。次のレッスンで詳しく扱います。

> [!CAUTION]
> 今後の演習でこの Codespace に戻ります。ひとまず、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers]で実行できるように構成されています。どちらの環境にも、スムーズに進めるために必要な前提条件がインストールされています。ローカルで実行する場合は、VS Code でクローンしたリポジトリを開き、プロンプトが表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成できました。また、Copilot クラウド エージェントと一緒に演習を進めるための Codespace の作成も開始しました。

## 次のステップ

次は、クラウド エージェントが従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みましょう。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
