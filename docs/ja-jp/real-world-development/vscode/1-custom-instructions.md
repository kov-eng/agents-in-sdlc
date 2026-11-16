---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: agent mode →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが、コード生成の指針を伝える[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を適用した状態で、フィルター機能の最初のデータ部分 (publishers ヘルパー) を生成します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルター機能のデータ部分を `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部と異なる場合があります。Copilot の出力は確定的ではありません。この演習の目的は、指示を更新した後の動作の「傾向」を確認することであり、出力を一字一句同じにすることではありません。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使うと、Copilot が適切な情報を受け取り、これらの開発手法に沿ってタスクを実行できるようになります。

### カスタム指示

カスタム指示を使うと、Copilot にコンテキストや設定を伝え、コーディング スタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コードに含めるコメントの形式などを指定すれば、より適切な提案やコード スニペットが得られます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot に送信するほとんどのチャットや CLI リクエストに関係する、プロジェクト レベルの情報を記載します。技術スタック、開発対象の概要、ベスト プラクティス、全般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に使用できます。TypeScript や Astro などの言語、または UI コンポーネントやユニット テストの作成などのタスクについてガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[指示ファイルの対応形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく説明することはこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な方法が示されています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスと例を示します。コードをどのように書くべきか、またどのように書くべきでないかを説明してください。

指示ファイルの作成方法は一つではありません。AI の使い方と同様に、プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを見ると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあることがわかります。
>
> Copilot に指示ファイルを作成させることもできます。各環境で手順は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。該当する場合は、利用中の環境に関するレッスンで説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot]では、指示ファイル、カスタム エージェントなどのリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスもあります。これらは、Copilot を使うあらゆる場面に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対パスで、指示の適用対象を決める glob を指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これらのパスは指示ファイルの場所からの相対パスです。指示を小さく再利用しやすいファイルに分け、Copilot がコードを生成するときに参考となる例を指定できます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントに関する要件はまだありません。そのため、生成されたヘルパーにはドキュメント コメントがありませんでした。次の手順で、このルールを追加します。

## 変更用ブランチを作成する

ここでは、変更用にブランチを作成するベスト プラクティスに従います。

1. 前の演習で使った codespace に戻ります。
2. 新しいターミナルを開くには、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送信します。その後、指示ファイルを更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブをすべて閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に伝えます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルとテストを含む複数ファイルにコード変更を適用します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的なため、指示がなくてもドキュメント コメントが追加される場合があります。その場合も問題ありません。重要なのは、指示を更新した後に動作の一貫性が高まることです。

## リポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供するファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する規則はありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc ドキュメント コメントを Copilot に追加させます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に置いて全体に適用することも、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` として作成することもできます。今回追加するドキュメント コメントの規則のような、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使うコードにその標準を適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同様に、ソース管理で管理します。変更をコミットして、ブランチをリポジトリにプッシュしましょう。

1. 新しいターミナル ウィンドウを開くには、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
2. ターミナルで次を実行し、指示の更新とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次を実行し、指示の更新とヘルパーの基盤に対して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルで次を実行し、ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、プルリクエストを作成し、バックログにあるドキュメント標準の issue と関連付けます。手動で作業することもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に任せましょう。issue を検索して、issue をクローズする PR を作成し、その後マージするように Copilot にプロンプトを送ります。

> [!NOTE]
> Copilot に GitHub 上の操作をさせるのは、この演習が初めてです。プロジェクト テンプレートでは GitHub のツールがあらかじめワークスペースに接続されているため、セットアップは不要です。Copilot がツールを初めて使うときに、VS Code から GitHub へのサインインを求められることがあります。プロンプトに従って許可してください。この接続の仕組み (Model Context Protocol) は、後の演習で学びます。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と新しい publishers ヘルパーの基盤の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作を実行するための **Allow** を求めたら、コマンドを確認し、適切な場合に **Allow** を選択します。
5. PR が作成されたら、次のプロンプトを使って PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作やシェル コマンドを実行するための **Allow** を求めたら、コマンドを確認し、適切な場合に **Allow** を選択します。

これで、GitHub Copilot を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- コード生成プロンプトを送信し、*既存の*指示で Copilot がどのようなコードを生成するか確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案コードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は、指示を活用する[agent mode][next-lesson]で、コードベース全体に新機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
