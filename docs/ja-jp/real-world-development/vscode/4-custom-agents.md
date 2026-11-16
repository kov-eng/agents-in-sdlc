---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリ内の `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の種類の作業を効果的に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供します。チームの専門知識を再利用可能なエージェントとして形式化できます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング手法に従うセキュリティ エージェント、テスト パターンの一貫性を保つテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタム エージェントと agent skills の違い

カスタム エージェントと [agent skills][agent-skills-concept]には、機能上の重なりがあります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。違いを簡潔に表すと、**カスタム エージェントは作業者、skills はツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、skills (場合によってはほかのエージェント) を使って作業を進めます。この演習のアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに従ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフロー用 skill やテストの実行・管理用 skill を呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの活用方法と同様に、使う環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダーク テーマではテキストと背景色のコントラストが不十分で、読みにくいと感じるユーザーがいることがフィードバックでわかりました。このアクセシビリティの課題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成しましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot をどのように導くか理解するため、内容を確認しましょう。

codespace に戻り、ターミナルを開いて、アクセシビリティの作業用に `main` から新しいブランチを作成して切り替えます。演習 3 の絞り込み PR は別にしておきます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、カスタム エージェントに必須です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成に関する主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタム エージェントが、Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールに応じたチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、**Agent** が選択されていることを確認します。未選択の場合は選択してください。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在のエージェントが表示されています。既定では **default** が選択されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルを編集します。編集内容は、パスと差分とともにチャット内に表示されます。
6. 編集が適用されると、エージェントが変更した作業ファイル一式を示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えます。作業中、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、作業中の変更を確認し、必要に応じて方向を調整します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。特定のタスクや分野に特化した AI アシスタントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、特定の作業を効率よく行うよう Copilot を導けます。

次の概念を確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は[エージェントの作業を監視して調整][next-lesson]します。変更を進行中に確認し、同じセッションでライト モードの切り替え機能も追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
