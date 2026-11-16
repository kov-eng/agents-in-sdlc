---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウド エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot クラウド エージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)**を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、クラウド エージェントがバックグラウンドでリポジトリを調べ、変更を加え、プルリクエストを作成します。その間、別の作業を続けられます。

この演習では、まずクラウド エージェントが従うカスタム指示を追加し、GitHub の issue を割り当てて実装させます。その後、カスタム エージェントをレビューして使い、アプローチを調整します。また、agents ダッシュボードでセッションを監視して誘導し、最後にプルリクエストをレビューして成果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | クラウド エージェントが従うカスタム指示を追加する |
| [2. クラウド エージェント][ex2] | 非同期エージェント | Copilot クラウド エージェントに issue を割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントをレビューして使う |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して誘導する |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で Copilot クラウド エージェントが有効であること

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は[GitHub Education][callout-student-plan-education]を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウド エージェントは **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 始めましょう

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
