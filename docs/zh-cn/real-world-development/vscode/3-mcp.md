---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建 pull request（PR）前，应确认它在浏览器中确实有效。无需亲自逐项操作应用，可以连接 **Playwright 模型上下文协议 (MCP) 服务器**，让 Copilot 驱动真实浏览器为你测试功能，然后发布分支并创建 pull request。

本练习将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建 pull request。

## 什么是模型上下文协议 (MCP)？

当智能体模式可以访问编辑器以外的资源时，它的能力会大幅增强。Copilot 使用模型上下文协议 (MCP) 与外部工具和服务通信，这是智能体与外部工具和服务交互的一种标准方式。

![示意图：智能体模式的内部工作方式，以及它如何与上下文、大语言模型 (LLM) 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为人工智能 (AI) 智能体提供与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，并负责管理 AI 智能体与外部工具（例如现有应用程序编程接口 (API) 或 npm 本地工具包）之间的通信。每个 MCP 服务器都提供一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，使 AI 智能体可以创建存储库、更新现有存储库，以及管理 issue 和 pull request。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，使 AI 智能体可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以提供不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助大家发现并为生态系统做出贡献。

> [!CAUTION]
> 出于安全考虑，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先仔细审查源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已为你配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

`github` 条目随项目模板提供，因此 Copilot 能在上一练习中读取待办 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 通过 HTTP 连接服务器，Copilot 首次使用其工具时需要使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件如下所示：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

远程 `github` 服务器与之不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志让 Playwright 在没有可见窗口的情况下运行浏览器。这是 codespace 中的必要设置，因为该环境没有用于显示浏览器的桌面。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此所需浏览器通常已安装。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，使服务器可以启动。

手动启动是可选的。如果跳过，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可驱动的真实浏览器。无需亲自操作应用来检查更改，智能体可以打开页面、导航、应用筛选并读取结果，然后总结观察内容。这是在不离开对话的情况下快速确认功能符合预期的方式。

Playwright MCP 服务器通过页面的[无障碍树][playwright-mcp-server]而不是截图进行操作。这意味着智能体根据有标签的结构化元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式一致。因此，快速功能检查也可以顺便进行基本的无障碍检查。

服务器已连接且应用正在运行后，请 Copilot 测试刚完成的筛选功能：

```text
使用 Playwright MCP 服务器，在 http://localhost:4321 打开正在运行的应用，并验证新添加的游戏筛选功能：

1. 打开游戏页面并记录列出了多少款游戏。
2. 应用类别筛选，并确认列表只显示该类别的游戏。
3. 清除筛选，再应用发行商筛选，并确认列表更新为该发行商的游戏。
4. 同时应用类别和发行商筛选，并确认结果同时符合两项条件。

报告每一步的观察结果，并指出任何不符合预期的行为。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行检查并报告发现。请将总结与 issue 中的验收标准进行对照。如果发现异常，请提出后续问题，或要求它在创建 pull request 前修复代码。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建 pull request

确认功能正常后，就可以创建 pull request（PR），让团队审查。先发布 `filtering-vscode` 分支。

1. 打开 codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建 pull request

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建 pull request。既然正在使用 GitHub Copilot，不妨让它代为创建。它可以查找相关 issue，并创建与 issue 关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉菜单中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   查找存储库中与按类别和发行商筛选相关的 issue。为当前分支创建新的 pull request，并将其关联到正确的 issue。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会要求使用 GitHub 身份验证，请按提示完成授权。
5. 留意 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接审查 pull request，但暂时**不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在浏览器中手动测试筛选功能。
- 发布了分支，并为筛选工作创建了 pull request。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
