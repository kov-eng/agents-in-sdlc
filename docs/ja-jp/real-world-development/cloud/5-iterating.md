---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが改善できる関連ゲーム機能を構築し、高コントラストとライト モードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要なら Copilot にフィードバックを送って、作業を改善しましょう。

### シナリオ

これまでに何度も確認したとおり、生成 AI が加わっても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って進めます。チーム全体にレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot クラウド エージェント

Copilot クラウド エージェントは非同期かつ監視なしでタスクを実行するため、安全性を保つためのセキュリティ制約が設けられています。たとえば、次のような制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコードに使用するブランチ**のみ**に限定されます。
- クラウド エージェントは GitHub Actions 内で動作し、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot クラウド エージェントが作成した最初のプルリクエスト (PR)、コードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot クラウド エージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、より具体的なタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が付いています。その場合は Copilot の作業が終わるまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc の doc コメントなど、更新されたコードを確認します。具体的な変更内容は状況によって異なります。

    変更を確認する際は、演習 2 の最初に確認したカスタム指示ファイルから反映された TSDoc の doc コメント、TypeScript の規則、コメント ヘッダーを確認します。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやりとりする方法は、結果を受け取るだけではありません。チーム メンバーに依頼するように、プルリクエストやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確定的ではないため、必ず通用する具体的な文章はありません。依頼したい内容の例を挙げます。

- 各コード ファイルの先頭に、ファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc の doc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. ドキュメントの変更を依頼するコメントを追加し、他のユーザーと同じように **@copilot** をメンションします。上記の例から選ぶか、コードベースに追加してほしいドキュメントについて別の案を入力します。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足するまで繰り返し改善できます。作業が完了したら、PR をドラフトからレビュー可能な状態に変更して、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、より具体的なタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック セクションにワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前回と同じように、Copilot クラウド エージェントに変更を依頼して反復できます。たとえば、表示する関連ゲームの数の調整や、コメント ヘッダーと TSDoc の doc コメントの追加を依頼できます (この issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブで新しいコメントを追加すれば、Copilot が内容を確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、より具体的なタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が付いています。その場合は、Copilot が作業を終えるまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装をレビューします。特に次の点を確認します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保持するためのローカル ストレージの使用方法。
   - 高コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーで高コントラスト モードとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot を導いたことがわかります。アクセシビリティに懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして、修正を依頼できます。

## 任意の演習 — さらに作業を委任する

クラウド エージェントには、実際のバックログ項目を任せ、自分は別の作業に取り組む方法が適しています。この方法に慣れるために、リポジトリにさらにいくつか issue を作成し、Copilot に割り当ててみましょう。次のような案があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウド エージェントのハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、クラウド エージェントの出力に反映される様子を学びました。
- **issue を Copilot クラウド エージェントに割り当て**、環境を準備し、計画を立て、非同期に作業する様子を確認しました。
- アクセシビリティ向けのカスタム エージェントを作成して使い、高コントラストとライト モードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、アクセシビリティ セッションを監視して作業中に誘導しました。
- **クラウド エージェントのプルリクエストをレビューして改善し**、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

クラウド エージェントのハーネスを完了しました。さらに学びたい場合は、他のハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、およびオプションの `/delegate` によるクラウド エージェントへの連携を使って、レビュー済みの変更を 2 つターミナルから完了します。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの issue をクラウド エージェントに割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウド エージェントのセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot クラウド エージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
