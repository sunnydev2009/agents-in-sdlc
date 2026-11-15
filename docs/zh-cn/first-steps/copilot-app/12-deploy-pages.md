---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查其 pull request，并验证 Space Quiz 是否已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问入口。Copilot 可以准备工作流并指导部署，但仍需由你审查更改并确认网站已上线。

在本课中，你将：

- 请 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和 pull request。
- 按照部署流程操作，并验证已发布的网站。

## 场景

Space Quiz 已经位于 GitHub 存储库中，但人们还需要一个简单的试用入口。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖，即可通过公开统一资源定位符 (URL) 访问。

## 请 Copilot 准备部署

> [!WARNING]
> 创建 pull request 可能会提交并推送代码。发送此提示前，请检查审批设置和目标存储库。如果选择 **Approve all**，不要期待系统再单独请求确认。

在 `space-quiz` 的 Copilot app 会话中，发送以下提示：

```plaintext
为此存储库准备 GitHub Pages 部署。如果需要，创建一个用于构建网站的 GitHub Actions 工作流，并在更改进入默认分支后将网站部署到 GitHub Pages。如果存储库中尚无 README，请添加一个，介绍此应用以及如何在本地运行它。创建包含这些更改的 pull request，并告知我其链接以及所需的 Pages 设置或权限。不要合并、启用自动合并、更改 Pages 设置或部署。完成后停止并等待我审查。
```

## 审查工作流和 pull request

1. 确认只有在存储库尚无 README 时才添加该文件。
2. 审查 pull request 中的更改，包括工作流的构建和部署步骤、触发条件及权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的 pull request 部署。
4. 要求修正任何问题，并审查更新后的差异。授权合并前，等待必需的 pull request 检查通过。

## 授权合并和部署

> [!WARNING]
> 以下提示授权智能体合并已审查的 pull request、更改存储库的 Pages 设置并发布网站。只有完成审查并确认存储库和 pull request 后，才发送此提示。审批设置可能允许这些操作不经再次确认直接执行。

将 `<PR_NUMBER>` 替换为已审查的 pull request 编号，然后单独发送以下提示：

```plaintext
我已审查此存储库中的 pull request <PR_NUMBER>，并授权合并和部署。使用 GitHub Actions 启用 GitHub Pages；仅在必需检查通过后合并该 pull request；并持续监控部署，直到成功。请报告已上线网站的最终 URL。如果你没有启用 Pages 或合并的权限，请准确说明我需要批准或执行的操作；在网站上线前，不要报告成功。
```

![三阶段示意图：已合并的 space-quiz pull request；GitHub Actions 中成功运行的 Deploy website 工作流，其中包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

1. 持续查看 GitHub Actions 部署运行情况，直到成功。
2. 打开报告中的 URL，确认 Space Quiz 可以正常加载。

如果智能体无法启用 Pages 或合并 pull request，请按照它提供的具体说明操作。只有部署成功且网站可以正常加载，任务才算完成。

## 总结和后续步骤

Space Quiz 现在已作为在线网站发布，你也亲自验证了部署，而不是只依赖智能体的报告。继续阅读[第 13 课：回顾和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
