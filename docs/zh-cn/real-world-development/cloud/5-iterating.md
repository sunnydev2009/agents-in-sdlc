---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在整个实验过程中，你与 GitHub Copilot 一起完成了多项改善用户体验和增加功能的任务。你请 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式开关的无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈，改进其工作。

### 场景

正如前面多次强调的，软件设计和 DevOps 的基本原则不会因生成式人工智能 (AI) 的加入而改变。始终需要审查生成的代码，并遵循常规 DevOps 流程。基于这一点，在为团队其他成员开启审查前，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能所做的更改。

## GitHub Copilot cloud agent 的安全性

由于 Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，对其工作分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，模型上下文协议 (MCP) 服务器也包括在内。

## 审查生成的文档

先在 GitHub.com 的标准 PR 界面中查看 GitHub Copilot cloud agent 创建的第一个 pull request（PR），其中包含代码文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 在 GitHub.com 上打开你的存储库。
2. 选择 **Pull Requests**，打开 pull request 列表。
3. 打开标题类似 **Add missing documentation** 或更完整描述的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待它完成工作，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。实际更改可能有所不同。浏览更改时，留意 TSDoc 文档注释、TypeScript 规范和注释头。这些来自练习 2 开始时审查的自定义说明文件。
6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应显示在 PR 的检查区域中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像标记其他团队成员一样，在 PR 评论或代码行内评论中标记 Copilot。Copilot 会读取评论，并启动新的会话来处理请求。由于结果具有非确定性，这里无法提供固定提示文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，要求修改生成的文档，并像标记用户一样标记 **@copilot**。可以使用上述建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，查看 Copilot 执行修改的过程。留意 Copilot 如何启动新会话来更新内容。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为准备审查状态，并合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 在 GitHub.com 上重新打开你的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完整描述的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应显示在 PR 的检查区域中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。和之前一样，可以迭代请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你尚未更新自定义说明）。与之前一样，可在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心中提出的浅色模式。

1. 在 GitHub.com 上重新打开你的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完整描述的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式对应的层叠样式表 (CSS) 或样式更改。
   - 无障碍属性（无障碍富互联网应用 (ARIA) 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应显示在 PR 的检查区域中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以切换到 Codespace 中的该分支，手动测试无障碍功能。在终端中运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认重新加载页面后设置仍然有效。

注意，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像前面一样在评论中标记 **@copilot** 请求更新。

## 可选练习——继续委托任务

Cloud agent 很适合处理待办事项，让你可以把精力放在其他工作上。为养成这种习惯，可以在存储库中创建更多 issue 并分配给 Copilot。以下是一些建议：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作环境的练习。在这些课程中，你：

- **检查了此存储库附带的自定义说明文件**，以便之后观察它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它异步设置环境、制定计划和执行任务。
- **创建并使用了自定义智能体**处理无障碍任务，添加高对比度和浅色模式开关。
- **使用 Copilot agents 页面作为任务控制中心**，监控并引导正在进行的无障碍会话。
- **审查并迭代 cloud agent 的 pull request**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 工作环境的练习。想继续探索，可查看其他工作环境：

- 🖥️ **[VS Code 工作环境](../../vscode/)**：直接在集成开发环境 (IDE) 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)**：在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端委托功能。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
