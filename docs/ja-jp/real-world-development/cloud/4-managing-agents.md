---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの 2 つの演習では、Copilot クラウド エージェントにユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを依頼しました。クラウド エージェントは非同期かつ自律的に動作しますが、タスクを監視することも重要です。

クラウド エージェントに割り当てたタスクを管理するためのツールは複数あります。たとえば GitHub.com の[agents ページ][agents-page]では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションを誘導したりできます。

このレッスンでは、次のことを行います。

- agents ページを使ってクラウド エージェントのタスクを監視します。
- 実行中のセッションを誘導し、機能の追加を依頼します。

## シナリオ

高コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加する良い機会だと気付きました。サイトのスタイルを更新し、切り替え機能を追加しているところなので、自然な拡張です。エージェントの作業を誘導して、高コントラスト モードに加えてライト モードも追加してもらいましょう。

## Copilot クラウド エージェントのタスクを確認する

Copilot クラウド エージェントに割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方にタスクの一覧が表示されます。Copilot に割り当てた次のようなタスクが表示されていることを確認します。
    - コードベースのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトに高コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。次のような操作が含まれる場合があります。
    - リポジトリからコードをチェックアウトする。
    - Copilot 用の環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するため、さまざまな処理を実行する。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## クラウド エージェントを誘導する

進行中のタスクを確認できたので、高コントラスト モードの作業中にライト モードの切り替え機能も追加するよう Copilot に依頼しましょう。

1. 高コントラスト モードを追加するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状態によって異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページで、作業中の Copilot セッションを誘導するダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業計画に反映することを確かめます。

## Copilot の作業を待つ

前回と同じように、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった手順を終えると、新しい依頼を作業の流れに反映します。

今回も数分かかります。これまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot クラウド エージェントのタスクを監視して誘導する中心的な場所である Copilot agents ページを確認しました。この mission control では、進行中および完了したタスクを追跡し、実行中の作業をレビューし、スコープの調整や追加の案内のためにセッションを誘導できます。

次の概念を確認しました。

- mission control と agents ページを使ってクラウド エージェントのタスクを監視する。
- 実行中のセッションを誘導して、機能の追加を依頼する。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプルリクエストを[反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
