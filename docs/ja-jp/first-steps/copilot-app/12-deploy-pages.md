---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを構成し、その pull request をレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Space Quiz を GitHub Pages にデプロイして、誰でもアクセスできるようにしましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の内容を学習します。

- Copilot に GitHub Pages デプロイの構成を依頼します。
- マージ前にワークフローと pull request をレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに公開 URL から利用できます。

## Copilot にデプロイを準備してもらう

> [!WARNING]
> pull request を作成すると、コードのコミットとプッシュが行われる場合があります。このプロンプトを送る前に、承認設定と送信先リポジトリを確認してください。**Approve all** が有効な場合、別途確認が求められるとは限りません。

Copilot app の `space-quiz` セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローと pull request をレビューする

1. README は、リポジトリに存在しない場合に限って追加されていることを確認します。
2. pull request の変更をレビューします。ワークフローのビルドとデプロイの手順、トリガー、権限を確認します。
3. ワークフローが未マージの pull request ではなく、既定のブランチからデプロイすることを確認します。
4. 必要な修正を依頼し、更新された差分をレビューします。マージを許可する前に、必須の pull request チェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトは、レビュー済み pull request のマージ、リポジトリの Pages 設定変更、サイトの公開をエージェントに許可します。レビューを終え、リポジトリと pull request を確認してから送信してください。承認設定によっては、追加の確認なしにこれらの操作が行われます。

`<PR_NUMBER>` をレビューした pull request の番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3段階の図。Space Quiz の pull request がマージされ、Build website、Upload artifact、Deploy to Pages の各ステップを含む GitHub Actions の Deploy website 実行が成功し、yourname.github.io/space-quiz/ で Space Quiz が公開されています。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを検証する

1. GitHub Actions のデプロイ実行を開き、成功するまで確認します。
2. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントに Pages の有効化や pull request のマージ権限がない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで完了とはみなしません。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告だけに頼らずデプロイを確認できました。[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
