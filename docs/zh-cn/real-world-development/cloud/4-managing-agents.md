---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 承担了三个改善用户体验和增加功能的任务。cloud agent 虽然以异步和自主方式工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭 pull request（PR）的智能体任务，查看它执行的操作，甚至在会话进行期间引导它。

本课将：

- 查看智能体页面，监控 cloud agent 任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也应该添加浅色模式。既然正在更新网站样式并添加切换功能，顺便加入浅色模式很合理。你将引导智能体，使其同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

先查看分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成指定任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和采用的方案而变化。

4. 同时留意右侧出现的 PR 面板。你可以在此查看 PR 和更改的文件，以便继续监控。

## 引导 cloud agent

了解当前活动任务后，请求 Copilot 在处理高对比度模式时也添加浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。看到类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   添加高对比度模式时，也请添加浅色模式。浅色模式也应有一个切换开关，让用户选择所需的显示模式。
   ```

   ![智能体页面中突出显示“Steer active session while Copilot is working”对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，然后将新要求纳入工作流程。

这同样需要几分钟。等待期间，可以暂停一下，回顾迄今学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。在此任务控制中心，可以跟踪所有正在进行和已完成的任务、审查工作，也可以重定向正在进行的任务以调整范围或提供额外指导。

你了解了：

- 使用任务控制中心和智能体页面监控 cloud agent 任务。
- 重定向正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代审查 Copilot 创建的 pull request][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
