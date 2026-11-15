---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "查看存储库说明，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在你已构建筛选功能，接下来将查看 Copilot 使用的说明、添加文档标准，并将标准应用于代码。

本课将：

- 了解存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TypeScript/JavaScript 文档注释规范 (TSDoc/JSDoc) 说明其用途、参数和返回值；如果存在可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应记录其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 根据这些实践执行任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码规范、库，甚至希望代码包含哪类注释。说明既可以适用于整个存储库，也可以针对特定类型的文件。

有两类说明文件：

- `.github/copilot-instructions.md`：每个存储库请求都会发送给 Copilot 的单个说明文件。内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 不同环境对其他说明格式的支持各不相同。使用特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

先了解入门项目已包含的说明文件，再进行修改，以便观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件，留意项目简介和编码指导。此文件适用于该存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中内容。注意其中有 Astro、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了用于确定适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分成较小、可复用的文件，并提供可供参考的示例。

## 更新说明以符合团队指南

现有文件是不错的起点，但仍缺少一条规范。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 遵循文档标准。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 请 Copilot 根据更新后的指导修改代码：

   ```plaintext
   我们刚刚更新了说明和代码指导。请将你生成的代码更新为符合这些指导。
   ```

4. 输入 `/diff`，查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确描述代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何获取说明文件中的上下文，并将新标准应用于功能。具体来说，你：

- 查看了存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
