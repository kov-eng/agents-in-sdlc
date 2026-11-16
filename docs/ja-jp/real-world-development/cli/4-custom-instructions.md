---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリング コードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。特定の方法でタスクを進める必要がある場合は、Copilot がそのガイダンスを参照できるようにしましょう。[指示ファイル][instruction-files]には、希望するコードの「内容」だけでなく、「構成方法」も記載します。フィルタリングを実装したところで、Copilot が使用した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる仕組みを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントでは、コードの内容を繰り返さず、目的や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値、および該当する場合は注入可能な `db` 引数を TSDoc/JSDoc で記載します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連コードが変更されたらコメントも最新の状態にします。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを進められるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストや希望を Copilot に与え、より適切な提案やコードを生成できるようにできます。希望するコーディング規則、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルには、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> 他の指示形式やサポート状況は、環境によって異なります。特定の形式を使う前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更を加える前に、スターター プロジェクトに含まれる指示ファイルを調べ、変更による影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。このリポジトリで Copilot を使うすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが指示の適用先となるファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。これにより、指示を小さな再利用可能なファイルに分け、コード生成の参考となる例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ不足があります。基本の `copilot-instructions.md` を変更して、新しく生成される TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** というセクションを見つけます。
2. 最後の箇条書きとして次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を読み込ませつつ、機能のコンテキストを保つために、フィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトした状態から、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に次のように依頼して、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正しく説明していることを確かめます。

## まとめと次のステップ

指示ファイルから Copilot CLI がコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルをカスタマイズして実行し、lint とテストが一貫して実行されるようにします。[次のレッスン][next-lesson]に進みましょう。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
