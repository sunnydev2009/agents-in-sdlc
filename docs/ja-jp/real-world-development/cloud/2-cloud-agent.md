---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト pull request (PR) を作成します。面倒な作業を任せて開発を加速し、より大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境をカスタマイズします。
- 操作を安全に実行する方法を確認します。
- 範囲を明確にした Issue の重要性を理解します。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを作った契約開発者は、ドキュメントを不十分な状態で残しました。実際には、ドキュメントがほとんどありません。最初の改善として、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのゲームを表示し、閲覧を続けられるようにします。洗練された実装はまだ必要ありません。ユーザー体験 (UX) の受け入れテストに使える状態を目指します。この作業は現在のブロッカーですが、優先度の高い Issue がほかにあります。

どちらも優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が別の作業に取り組んでいる間に非同期で処理できます。その後、成果をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると、Copilot は進行状況を追跡するドラフト pull request を作成し、環境をセットアップしてタスクを開始します。実行中または完了後にセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作にも複雑な側面があるため、[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot への影響を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、ここで確認してください。cloud agent に作業を割り当てる前に、リポジトリに用意された指示ファイルを読み取り専用で確認しておくと、後でその影響を見つけやすくなります。

GitHub Web UI または起動中の codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter が `**/*.test.ts` ファイルを対象にしていることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された pull request に TypeScript documentation (TSDoc) のドキュメントコメント、TypeScript の規約、コメントヘッダーがあるか確認します。後の演習で PR をレビューするときに、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードの作成には、誰が作業する場合でも、通常は環境の準備とセットアップスクリプトの実行が必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を用意します。作業開始前に実行する[専用セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、必要な開発ツールや依存関係を利用できるようにできます。このラボでは、学習を円滑に進めるため、事前に構成済みです。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンド (E2E) テスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
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

      # ビルドとテストでデータを使えるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同様の形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、古い情報しかない、またはドキュメントがまったくないプロジェクトもあります。技術的負債として放置されると、生産性が低下し、コードベースの保守や新しい開発者の参加が難しくなります。Copilot はドキュメントの作成が得意なので、この作業は cloud agent に割り当てるのに適しています。バックグラウンドで必要なドキュメントを作成し、後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような形式です。一覧に表示されない場合は少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作成した todo リストが表示されます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を準備し (先ほど説明したワークフローを実行し)、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. リストと、Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したという更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターが表示されるよう、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot がコードを調査して状況を把握し、計画を考えてコードを作成する様子を確認します。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、開発者がほかの作業に集中できるようにすることです。この機能を活用するため、もう1つのタスクも cloud agent に割り当て、その間にアプリケーションの機能を追加する作業に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の1つは作業を分けられることです。デザインチーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を始めたことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue の作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常時監視する必要はありません。完了後にレビューします。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を確認しました。Issue を Copilot に割り当てて、非同期に作業させることができます。技術的負債への対処、新機能の作成、別のフレームワークからのコード移行などに活用できます。

次の内容を確認しました。

- コード生成環境をカスタマイズします。
- 操作を安全に実行する方法を確認します。
- 範囲を明確にした Issue の重要性を理解します。
- Issue を Copilot に割り当てます。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使用します。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
