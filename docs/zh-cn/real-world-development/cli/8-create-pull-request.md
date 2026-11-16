---
title: "课程 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支上。现在一起审查这些内容并创建拉取请求。你之前亲自合并了星级评分拉取请求（PR），这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会把功能、说明更新、技能更新和 QA 代理拆分为几个独立 PR。为简化本工作坊，你将筛选功能和质量工作流都保留在同一个对话和分支中，并通过一个 PR 提交所有内容。

本课将学习如何：

- 了解 Agent Merge 以及它如何自动化 PR 生命周期。
- 检查完整的功能变更和验证证据。
- 创建筛选功能 PR。
- 审查后才启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化剩余的 PR 工作，同时让开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决 PR 当前受阻的事项，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时变基；之后，如果存储库允许，它会启用 GitHub 自动合并。

到目前为止，你都是自行选择 **合并拉取请求**。Agent Merge 可以代为处理这项工作。启用 Agent Merge 前，请先审查变更并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码都已完成，现在一起审查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会跟进 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请查看报告的原因，并在存储库允许时按审查流程手动完成合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 以及它如何自动化合并生命周期。
- 检查了完整的功能变更和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多有用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
