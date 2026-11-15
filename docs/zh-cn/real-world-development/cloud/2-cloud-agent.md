---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码和添加功能等任务。任务完成后，它会创建一个草稿 pull request (PR)，供开发者审查。这样可以将繁琐任务委派出去，加快开发进度，让开发者专注于更重要的工作。

本课将使用 Copilot cloud agent 探索以下内容：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档——事实上，几乎完全没有文档。首先，团队希望为应用中所有导出的函数添加 TypeScript 文档注释规范 (TSDoc) 注释。

此外，设计团队准备改善游戏发现体验。他们希望在每款游戏的详情页上显示相关游戏，即同一类别中的其他游戏。暂时不需要完善的实现，只需要可用于验收测试用户体验的版本。目前这项工作受到其他更高优先级任务的阻碍。

这两项任务都可能被不断推迟，很适合分配给 Copilot cloud agent。它可以异步处理任务，让开发者继续其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿 pull request 来跟踪进度、设置环境并开始工作。任务进行期间或完成后，都可以查看 Copilot 会话。解决方案准备好供审查时，Copilot 会在 pull request 中标记你。

## 明确界定任务范围的重要性

GitHub Copilot 没有魔法，不能替你省去思考。即使看似简单的操作，也可能包含复杂细节，因此[应仔细界定交给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作人工智能 (AI) 结对编程助手：分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

之前的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没完成，现在可以快速查看。分配任务前，先只读查看存储库中已有的说明文件，稍后便能发现它们的影响。

在 GitHub 网页界面或已运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些说明限定为 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` issue 分配给 cloud agent 时，留意生成的 pull request 中的 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自上述说明文件。后续练习审查 PR 时还会再次检查。

## 为 Copilot cloud agent 设置开发环境

编写代码通常需要特定环境并运行一些设置脚本，以确保项目状态正常。将任务分配给 Copilot 也是如此，因为它的工作方式类似软件工程师。

Cloud agent 使用 [GitHub Actions][github-actions] 环境执行任务。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]自定义环境，在 Copilot 开始工作前安装必要的开发工具和依赖项。实验已预先配置此工作流，以便顺利完成流程并进行学习。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的单体 Astro 应用 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅为步骤设置所需的权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件类似，但有几个要点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 pull request 前，GitHub Actions 会先执行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，这样可以先验证工作流，而不必等 Copilot 启动它。

## 添加文档

尽管大家都了解文档的重要性，许多项目仍然存在信息过时或缺失的问题。这类技术债务经常得不到处理，降低工作效率，也让维护代码库或引入新开发者变得更加困难。Copilot 擅长生成文档，因此很适合将这项任务分配给 Copilot cloud agent。它会在后台生成所需文档，稍后可回来审查其成果。

1. 在新浏览器标签页中打开 GitHub.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   我们组织要求在适当情况下为函数和方法添加 TSDoc 文档注释。不幸的是，近期更新没有遵守这一标准。我们需要更新现有代码，在文档注释能够阐明行为的地方添加注释。
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 pull request（PR），标题可能类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看待办列表以及 Copilot 准备完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解现状、暂停思考并决定合适计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一就是异步执行任务，让我们能够专注于其他工作。接下来利用这一点：先向 Copilot cloud agent 分配另一项任务，然后编写代码为应用添加功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot cloud agent 的一个重要优势是能够分配并行任务，让你专注于一项工作，同时让它处理另一项工作。即使为设计团队添加相关游戏区域不会花费太久，这段时间也可以用于其他任务。现在将此工作分配给 Copilot cloud agent。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队想探索用户体验并进行验收测试。需求如下：

   - 在 `src/lib/` 中添加一个数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，并排除该游戏本身
   - 在游戏详情页添加一个使用此辅助函数的“Related games”区域
   - 处理没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前确保所有测试通过
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，issue 中的第一条评论旁边应出现 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。Copilot cloud agent 的工作方式类似软件工程师，因此无需一直监控；等它完成后再审查即可。现在转而了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent 可以将 issue 分配给 Copilot，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

Copilot cloud agent 正在后台工作。接下来创建并使用自定义智能体。Copilot cloud agent [也可以使用模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并支持我们在前面模块中探索过的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
