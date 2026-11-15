---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建一个 QA 自定义智能体，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright 模型上下文协议 (MCP) 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，定义清晰的质量保证 (QA) 流程。

本课将：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选 issue 审查其发现。

## 场景

在创建 pull request（PR）之前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

本课程中使用的自定义项各有不同用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告发现。它不会取代其他配置，也不需要另开对话。

## 创建 QA 配置文件

在打开功能 PR 之前，请 Copilot 创建一个可复用的 QA 配置文件，定义 QA 执行的检查及其必须遵守的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   创建一个名为 QA 的自定义智能体，保存到 .github/agents/qa.agent.md。它应根据 issue 和已商定的需求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它将每项需求标记为通过、失败或受阻，并提供支持证据。修改实现代码前必须征求同意；不得提交更改或创建 pull request。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先检查其配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这可以避免你只想验证功能时，智能体却以不完整或过宽的权限修改功能。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段；`name` 是可选字段，但有助于显示明确的智能体名称。
3. 阅读配置文件中的指令，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建 pull request。
5. 如果生成的配置文件缺少上述职责或边界，请先要求默认智能体修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一个筛选对话并选择 QA，以便它使用已有的 issue 和规划决定。

1. 使用以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中。它仍然可用，上述命令会启用它。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   根据 issue 和计划中的决定审查筛选功能。它可以准备好创建 PR 了吗？
   ```

3. QA 智能体开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你为工作流添加了可复用的专家角色，并审查了其工作。本课中，你：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选 issue 审查其发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好供审查。接下来，将它们整合到[功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
