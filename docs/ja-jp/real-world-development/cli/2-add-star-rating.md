---
title: "レッスン 2 - 星評価を追加する: 手軽な改善"
description: "Copilot CLI でゲーム カードを小さく変更し、ポート フォワーディングされたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに最初の変更を加えましょう。小さな変更から始めます。ゲーム データには星評価がすでにありますが、ホーム ページのゲーム カードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を開始します。
- プロジェクトへの小さな変更をエージェントに依頼します。
- `/diff` で変更をレビューします。
- アプリを実行し、ポート フォワーディングされたブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定できます。ゲーム詳細ページにはすでに評価が表示されていますが、ホーム ページのゲーム カードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されていません。ウォームアップとして、エージェントに既存の評価を各カードに表示してもらいましょう。最初のセッションに適した、小さく独立した変更です。

## 会話の構成

**会話**は Copilot CLI でタスクに取り組む場所です。Copilot アプリとは異なり、通常の CLI 会話では、専用の worktree を作成せず、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話を後から再開できます。ファイルやブランチはディスク上の通常の Git 状態のままです。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装を依頼します。

1. Codespace に戻ります。
2. まだターミナルを開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ開いていない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を実行し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを特定し、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は各ゲーム カードに数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態がカバーされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> すべての生成 AI ツールと同様、Copilot は確定的ではなく、確率的に動作します。そのため、実際に生成されるコードは異なる場合があります。特定の実装と比較するのではなく、動作をレビューしてください。

## 変更を確認する

コードを読んだだけで動作すると判断するべきではありません。Copilot に Web サイトを起動してもらい、Codespaces でポート フォワーディングされたブラウザーから更新された UI を確認します。

1. Copilot に次のように依頼して、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階評価が表示されていることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう次のように依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は新しいコードをプロジェクトに取り込むプルリクエスト (PR) を作成します。

1. 既定のエージェントに次のように依頼して、変更をコミットします。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。PR を作成すると、Copilot CLI が既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースできました。具体的には、次のことを行いました。

- 機能ブランチで Copilot の会話を開始しました。
- エージェントにゲーム カードへの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、ポート フォワーディングされたブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、issue から始めて Plan モードと Autopilot モードを使い、より大きな機能を作成します。[次のレッスン][next-lesson]に進みましょう。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
