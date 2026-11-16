---
title: "演習 1 - カスタム指示 (クラウド エージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウド エージェント →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot に事前知識を与えたい場合は、そのコンテキストを参照できるようにしましょう。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」実行してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの「変更前と変更後」を比較しません。Copilot クラウド エージェントは GitHub issue を使って非同期に動作するため、リアルタイムで並べて効果を示すのが難しいからです。ハーネスの後半でクラウド エージェントが作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには必ずユニット テストを用意します。
- UI はダーク モードで、現代的な印象にします。
- コードのドキュメントには TSDoc の doc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストや希望を Copilot に与え、より適切な提案やコードを生成できるようにできます。希望するコーディング規則、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot に送るチャットや CLI のリクエストのほとんどに関係する、プロジェクト レベルの情報を記載します。たとえば、技術スタック、開発対象の概要、ベスト プラクティス、全体的なガイダンスなどです。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に使用できます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加などに向けたガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウド エージェントは、リポジトリ レベルのファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、別の規約に基づく指示ファイルにも対応しています。[対応する指示ファイル形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく扱うことはこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な構成を示しています。基本方針は次のとおりです。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイル種類や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスを記載します。コードの良い例と悪い例を示します。

指示ファイルの作成方法に決まった正解はありません。AI の使い方と同様に、試しながらプロジェクトに合った方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意しましょう。このプロジェクトのファイルを見ると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があることがわかります。
>
> Copilot に指示ファイルの生成を依頼することもできます。各環境で方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。該当するレッスンで、必要に応じて説明します。
>
> テンプレートや出発点を探していますか？指示ファイル、カスタム エージェントなどのリソースをまとめたリポジトリ、[awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリにある指示ファイルを確認しましょう。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** の指針もあります。これらは Copilot を使うすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示を適用するファイルを指定する glob です。ここでは、たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` など他の指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示を小さな再利用可能なファイルに分け、コード生成時に参考にできる例を Copilot に示せます (ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc コメントとファイル ヘッダーの規則を追加します。

## 新しいリポジトリ標準を追加する

この演習で行う編集は、コード内のドキュメントに TSDoc の doc コメントとファイル レベルのコメント ヘッダーを必須にする、プロジェクト全体のルールの追加です。クラウド エージェントは、このハーネスの後半で割り当てる issue に取り組む際、このルールを参照します。

編集する前に、作業用のブランチを作成します (クラウド エージェントは issue の対象ブランチから指示を読み込みますが、変更を整理してコミットするためです)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準として TSDoc の doc コメントとファイル ヘッダーを記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。ここにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントのルールはありません。そのため、生成されたヘルパーに doc コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加して、ファイル コメント ヘッダーと TSDoc の doc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは、グローバルな指針を記載するリポジトリ レベルの `.github/copilot-instructions.md` や、言語、ファイルの種類、タスクに特化した `*.instructions.md` ファイルとして作成できます。今回追加した doc コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

クラウド エージェントは、issue の対象ブランチから指示ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、クラウド エージェントに指示を適用するには、変更を `main` に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で issue を割り当てるとき、クラウド エージェントがこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` にコミットしてもかまいません。ブランチを使う手順は、実際のプロジェクトで行う方法を再現しています。

## まとめと次のステップ

このプロジェクトの指示ファイルによって Copilot がコンテキストを取得する仕組みを確認し、リポジトリ全体に適用する標準を追加しました。残りのハーネスでクラウド エージェントが作成するプルリクエストをレビューするときに、この標準が反映されているか確認できます。

次は、Copilot クラウド エージェントに[最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
