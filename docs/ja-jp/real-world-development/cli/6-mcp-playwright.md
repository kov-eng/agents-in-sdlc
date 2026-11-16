---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または構成し、ブラウザーからフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、Copilot が使える自動化機能の追加などが必要です。そこで MCP サーバーが役立ちます。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを学びます。
- Playwright MCP サーバーが利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように開発中の Web サイトを使えるようにし、期待どおりの変更かどうかをより自動的に確かめたいと考えています。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post]を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部ツールやサービスとリアルタイムにやりとりし、最新情報へのアクセスや操作の実行ができます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスを仲介する MCP サーバーを通じてアクセスします。各 MCP サーバーが、AI エージェントから利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

他にも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献できるように[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。使用する前にソース コードをレビューし、公開元を確認して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーを利用できない場合は追加して、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力してから、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。次は、Copilot が利用できる Playwright MCP サーバーを使って同じように確認します。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に指示します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自分で MCP サーバーを見つけて使うため、サーバーの使用を明示する必要はありません。ただし、使用するサーバーがわかっている場合は、プロンプトで示すとよいでしょう。より一貫した結果につながり、トークンも節約できます。

2. ブラウザーで機能を確認するため、Copilot が実行する手順を確認します。
3. 報告を読み、すべてが期待どおりに動くか確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認できました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- 利用できない場合に Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、スキルとブラウザー ツールを組み合わせる QA カスタム エージェントを[作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
