---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode を使って絞り込み機能を作成しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーで絞り込み機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、絞り込み機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エディターの外部にアクセスできると、agent mode はさらに強力になります。Copilot がそれを実現するために使うのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部のツールやサービスとリアルタイムにやり取りできます。その結果、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールとリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて利用できます。MCP サーバーは、AI エージェントと外部ツール (既存の API や NPM パッケージなどのローカル ツール) との通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースのセットを提供します。

よく利用されている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub の [MCP レジストリ](https://github.com/mcp)で、エコシステム内の MCP サーバーを検索したり、貢献したりできます。

> [!CAUTION]
> セキュリティの観点では、MCP サーバーをほかの依存関係と同様に扱ってください。使用する前に、ソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。今回の機能をテストするために使用します。

1. `.vscode/mcp.json` に `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモート サーバーの `github` とは異なり、`playwright` はローカル サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright はウィンドウを表示せずにブラウザーを実行します。デスクトップ画面がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright を使っているため、必要なブラウザーはすでにインストールされていることがほとんどです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行させてから、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がそのサーバーのツールを初めて必要としたときに、VS Code が起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、構成を確認し、信頼することを選択してサーバーを起動します。

手動で起動する必要はありません。手順を省略すると、次のステップで Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼の確認を求めます。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。ユーザー自身がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を読み取って報告できます。会話を続けたまま機能の動作を確認できるため、すばやくテストできます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目などのラベル付き要素を構造化してエージェントに渡します。支援技術と同じように情報を扱うため、簡単な機能テストでアクセシビリティも確認できます。

サーバーを接続してアプリを実行した状態で、作成した絞り込み機能をテストするよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各ステップを実行して結果を報告します。issue の受け入れ条件と照らし合わせて報告内容を確認してください。問題がある場合は、PR を作成する前に追加の質問をしたり、コードを修正するよう依頼したりします。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリを実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行させてから再度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認できたら、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) を使うなど、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も任せましょう。関連する issue を見つけ、PR と関連付けてくれます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるように、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のように Copilot に依頼して PR を作成します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して操作を実行できるようにします。GitHub ツールを初めて使うときは、GitHub の認証を求められることがあります。プロンプトに従って許可してください。
5. Copilot が issue を検索し、適切なものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで絞り込み機能を手動テストするためにサーバーを使いました。
- ブランチを公開し、絞り込み機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の一つにすぎません。次は、アクセシビリティの確認など、特定のタスクを効率化する[カスタム エージェントを作成][next-lesson]します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
