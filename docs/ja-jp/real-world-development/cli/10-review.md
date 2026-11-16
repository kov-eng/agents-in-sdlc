---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、シードされたフィルタリングの issue を見つけました。
- 星評価を追加し、ポート フォワーディングされたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始めて、Plan モードで方針を定め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの確認結果を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge によるフィルタリング PR のマージを許可しました。
- コンテキスト、モデル、共有、オプションのクラウドへの委任に使うスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップには、それぞれ更新済みの `main` から作成した別のブランチに、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と、評価がない場合の状態をゲーム カードに明示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用します。`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡略化するため、この作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で報告しました。QA は、その結果に要件とテスト カバレッジのレビューをまとめて PR の確認に使いました。

追加するテストは、実際に不足している部分を補うものでなければなりません。追加のテストが不要と判断される QA 実行も、適切な場合があります。マージを許可する前にコードと根拠をレビューし、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の質に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門家の役割を定義します。MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合わせて**モードとモデル**を選択します。構築前に方針を検討するには **Plan**、対象を限定した変更を一緒に進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

ツールやインフラストラクチャだけでなく、コンテキストも重要です。何を、なぜ、どのように構築してほしいかを明確に伝えることで、結果が大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI には、他にも便利な機能があります。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを独立させます。
- `/delegate` でタスクを Copilot クラウド エージェントに送信します。

## 次のステップ

ツールを使い続けることが、スキルを身に付ける一番の方法です。実運用のコード、趣味のコード、長年作ろうと思っていた小さなアプリなどで活用してください。学んだことをチームと共有し、チームからも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みましょう。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot クラウド エージェントのワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
