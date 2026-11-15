---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并通过第一个 pull request 合并它。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始修改项目了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示它。请智能体显示评分、审查更改，并将其作为你的第一个 pull request 合并。

在本课中，你将：

- 在功能分支上开始一段专注于当前任务的 Copilot 对话。
- 请智能体对项目进行小幅修改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个 pull request。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，并且详情页已经显示评分。不过，主页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，请智能体在每张卡片上显示现有评分。这是一个小而独立的更改，很适合作为首次会话任务。

## 对话的组成

**对话**是你使用 Copilot CLI 完成任务的工作区。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，不会创建专用工作树。保存的对话可以让你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你的提示和智能体的回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

先开始一个新对话，着手实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 开始新会话，然后按 <kbd>Enter</kbd>。
5. 使用以下提示请求更改：

   ```plaintext
   在列表页的游戏卡片上显示每款游戏的 starRating（满分 5 分）。如果评分为 null，则显示 "No rating yet"。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并创建所需代码。现在，你已使用 Copilot CLI 添加了一项功能。

## 审查差异

所有人工智能 (AI) 生成的更改在合并前都应经过审查，即使更改很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff`，检查每个已更改的文件。
2. 确认游戏卡片在评分存在时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复失败项。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此实际代码可能不同。审查行为，不要期待某一种固定实现。

## 检查更改

不能只阅读代码就假定它能正常工作。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面（UI）。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分的更改。告诉我统一资源定位符 (URL)，并让服务器继续运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个 pull request

功能已完成，现在可以创建 pull request（PR），将新代码合并到项目中。

1. 请默认智能体提交更改：

   ```plaintext
   使用恰当的提交消息提交已审查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 创建 pull request（PR）时会推送现有提交，并显示其统一资源定位符 (URL)。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个更改。具体来说，你：

- 在功能分支上开始了一段专注于当前任务的 Copilot 对话。
- 指示智能体修改游戏卡片。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个 pull request。

接下来，从筛选 issue 入手，使用 Plan 和 Autopilot 模式构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
