---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数ファイルの変更やテストの実行が必要です。開発者は、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更漏れのファイルを見つけて、さらに更新するという作業を繰り返します。作業は尽きません。

そんなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者のように既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった誤りを自動的に修正します。Agent Mode を使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内でタスクを実行するため、すばやいフィードバックと対話が可能です。cloud agent はチームの一員のように非同期で作業し、issue やプルリクエストを通じてやり取りするプログラミング パートナーです。

この演習では、次の方法を学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを調整して変更する方法。
- GitHub Copilot Agent Mode を使って、UI とデータ レイヤーにまたがる新機能を実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前に確認する方法。

## シナリオ

ゲームの数が増えてきたため、ユーザーがカテゴリやパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。この演習では、Copilot Agent Mode を使って残りのデータ レイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に配置し、インタラクティブな機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace 上で実行できます。

1. codespace に戻ります。前の演習が終わっていれば、`main` に戻っているはずです。
2. Codespace 内で新しいターミナル ウィンドウを開きます。<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. 絞り込み機能の作業用に、新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じてください。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を開くには、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながらアドレスを選択します。

> [!NOTE]
> codespace では、Codespace ターミナルに表示された localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Web サーバーをホストしている codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると詳細ページが開き、タイトル、説明、パブリッシャー、カテゴリなどが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの最初の実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップしたときに issue のバックログが自動作成されています。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** はファイルをまたぐ変更を自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップを作成したときに Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、快適に学べるようにするためです。ただし、LLM の出力は確率的なため、ワークショップの記載とは異なる提案が表示されることがあります。これは通常の動作であり、想定内です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を基準に、優先順位も付けていることを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを伝えるために使う指示ファイルを確認します。Web サイトに機能を追加する方法のコンテキストを含む [ユーザー インターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使用します。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類ごとの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルでは、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用しやすい小さなファイルに分けたり、例やテンプレートを参照させたりできます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つを別々に更新する必要があります。

- データ レイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整します。
- 絞り込み動作のテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

また、すべてのテストを実行して合格してから、コードベースにマージする必要があります。Copilot Agent Mode にこれらの作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連するファイルを特定する様子を確認します。データ レイヤーのヘルパー、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、タスクの複雑さやコードベースによって数分かかる場合があります。その間、エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業中は選択せず、完了を待ってください。作業が終わると、すべての変更に対して **Keep** または **Undo** が表示されます。

7. Copilot のプロンプトに従い、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示されているスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了してエラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode とのチャット セッションの完了例を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ず確認してください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに新機能を追加しました。次の方法を学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる新機能を実装する方法。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルを調整して変更する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前に確認する方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 追加の演習 — ページングを実装する

ゲームの一覧が増えるとページングが必要になります。この演習で学んだことを使い、Copilot にページング機能の実装を依頼しましょう。コードを書く際には、次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ユーザーがページ サイズを選択できるようにするか、値を固定するかなど、ページングの方式を検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- Copilot に変更を依頼したりコンテキストを提供したりして、何度かやり取りする場合があります。Copilot を使う際の通常の進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
