---
title: "演習 2 - GitHub Copilot クラウド エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがあるかもしれません。GitHub Copilot のクラウド エージェントは、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようになります。

Copilot クラウド エージェントを使って、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- issue のスコープを明確にすることの重要性を理解する。
- Copilot に issue を割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを適切に整備しなかったため、ドキュメントがまったくありません。まず、アプリケーションのすべてのエクスポート関数に TSDoc の doc コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。現在は作業の妨げになっていますが、他にも優先度の高い issue があります。

どちらも優先順位が下がりやすいタスクの例であり、Copilot クラウド エージェントに割り当てるのに適しています。クラウド エージェントに非同期で作業させれば、開発者は別のタスクに集中し、あとで戻って作業をレビューし、期待どおりか確認できます。

## GitHub Copilot クラウド エージェントの紹介

[GitHub Copilot クラウド エージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同様にバックグラウンドでタスクを実行します。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取りかかります。処理中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストであなたに通知します。

## 適切なスコープの指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot クラウド エージェントに依頼するタスクのスコープ][cloud-agent-best-practices]を慎重に決めましょう。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、適宜調整してください。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示と、それが Copilot を導く仕組みを確認しました。カスタム指示を実際に試したことがあれば、`.github/copilot-instructions.md` が機能することを確認できたでしょう。まだであれば、ここで簡単に確認してください。Copilot クラウド エージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み、後でその影響を確認できるようにします。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規則と TSDoc の doc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、指示の適用先が `**/*.test.ts` ファイルに限定されていることを確認します。

次のセクションで `Code lacks documentation` issue をクラウド エージェントに割り当てます。生成されたプルリクエストに TSDoc の doc コメント、TypeScript の規則、コメント ヘッダーが含まれているか確認してください。後の演習で PR をレビューするときにも、改めて確認します。

## Copilot クラウド エージェントの開発環境を設定する

誰が作業する場合でも、コードを作成するには通常、特定の環境やセットアップ スクリプトが必要です。タスクを Copilot に割り当てる場合も同じです。Copilot はソフトウェア エンジニアのように作業します。

クラウド エージェントは、作業環境に [GitHub Actions][github-actions]を使います。作業開始前に実行する[専用のセットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、環境をカスタマイズして、必要な開発ツールや依存関係を利用できるようになります。このラボの流れを簡単にし、学習機会を提供するために、事前に設定されています。単一の Astro アプリで使う Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *この手順*に必要な最低限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古いままのプロジェクトや、ドキュメントがまったくないプロジェクトも少なくありません。このような技術的負債が放置されると、生産性が低下し、コードベースの保守や新しい開発者の参加が難しくなります。Copilot はドキュメントの作成に役立つため、このタスクは Copilot クラウド エージェントに依頼するのに適しています。バックグラウンドで必要なドキュメントを作成します。後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行します)、プロジェクトを分析して、課題への最適な取り組み方を検討しています。

13. リストをレビューし、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ウィンドウの再読み込みが必要な場合があります。

16. 実行中のセッションをスクロールして、Copilot の作業を確認します。コードを調べて状態を理解し、適切な計画を検討してからコードを作成する様子を確認できます。

完了まで数分かかるでしょう。Copilot クラウド エージェントの主な目的の 1 つは、タスクを非同期で実行し、他の作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクをクラウド エージェントに割り当てた後、アプリケーションに機能を追加するコードを作成します。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot クラウド エージェントの大きな利点は、作業を分担できることです。デザイン チーム向けの関連ゲームのセクションは、必ずしも長時間の作業ではありませんが、その時間を別の作業に使えます。Copilot クラウド エージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![issue で目の絵文字を使って作業中であることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。Copilot クラウド エージェントはソフトウェア エンジニアと同じように動作するため、常時監視する必要はなく、完了後にレビューすればよいでしょう。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot クラウド エージェント][copilot-agents]について学びました。issue を Copilot に割り当て、非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

次の概念を確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- issue のスコープを明確にすることの重要性を理解する。
- Copilot に issue を割り当てる。

クラウド エージェントがバックグラウンドで作業している間に、次はカスタム エージェントの作成と使用を確認します。[Copilot クラウド エージェントは MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム指示にも対応しています。

## リソース

- [Copilot クラウド エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウド エージェントのセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
