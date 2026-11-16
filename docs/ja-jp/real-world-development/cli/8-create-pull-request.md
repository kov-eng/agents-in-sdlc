---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。すべてをまとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルタリングと品質ワークフローの作業を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能の変更全体と検証結果を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能のワークフロー全体を通じて、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys は、マージを許可する権限を開発者が保持したまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge の紹介

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に取り組みます。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要な場合のリベースなどを行います。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

ここまでは自分で **Merge pull request** を選択してきました。Agent Merge にこの作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューし、問題なくマージできると判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューし、PR を作成して、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 選択肢から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. `PR #14 was squash-merged successfully.` のようなメッセージが Copilot から表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリの設定で許可されていればレビュー済み PR を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの複数の部分を自動化しました。具体的には、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証結果を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、オプションのクラウドへの委任に使える[便利な Copilot CLI のスラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
