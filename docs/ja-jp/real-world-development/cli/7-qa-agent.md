---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザーからフィルタリングを確認しました。次は、明確な QA プロセスを定義したカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成してレビューします。
- QA エージェントを選択し、フィルタリングの issue に照らして報告を確認します。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門家向け設定です。プロファイルにはエージェントの目的、指示、使用可能なツールを記載します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズは、それぞれ異なる役割を持ちます。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、結果を報告する方法を Copilot に伝えます。カスタム エージェントはこれらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。QA が実施するチェックと、その境界を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のように依頼し、新しいカスタム エージェントを作成します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと境界を設定したことを確認します。エージェントが確認だけを求められているのに、不完全な指示や広すぎる権限で機能を変更するのを防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名がわかりやすくなります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットせずプルリクエストも開かないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に既定のエージェントにプロファイルを修正するよう依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら同じフィルタリングの会話を再開し、QA を選択して、会話に含まれる issue と計画の決定事項を活用できるようにします。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、報告を確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門家の役割を追加し、その成果をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの issue に照らして報告を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、これらを機能 PR にまとめ、Agent Merge を使います。[次のレッスン][next-lesson]に進みましょう。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
