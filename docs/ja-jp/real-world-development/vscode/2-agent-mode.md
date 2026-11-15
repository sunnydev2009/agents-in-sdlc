---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。統合開発環境 (IDE) で開発者は必要なファイルを探し、変更し、テストを実行して、問題を修正します。変更漏れに気付けば、さらにファイルを更新します。作業は続いていきます。

このような場面で Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業するよう設計されています。既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチームメンバーのように非同期で作業し、Issue と pull request を通じてやり取りします。

この演習では、次の内容を学習します。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する仕組みを確認します。
- GitHub Copilot Agent Mode でユーザーインターフェイス (UI) とデータレイヤーにまたがる新機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲーム一覧が増えてきたため、利用者がカテゴリーとパブリッシャーで絞り込めるようにします。前の演習でパブリッシャーヘルパーを追加したので、残りのデータレイヤー、UI、テストを Copilot Agent Mode で完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者向けテーマのボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示します。サイトは単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う小さなスコープ付き Astro `<script>` を使います。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習を終えて `main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL のバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くブラウザーウィンドウの表示を促すダイアログが開いた場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace では、ターミナルで localhost の URL を選択すると `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーをホストする codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示させます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** は自律的に計画し、複数のファイルを変更して、ターミナルコマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨する意図はありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順でも指定しています。提案の一貫性を高め、スムーズに進めるためです。ただし、大規模言語モデル (LLM) は確率的に動作するため、説明と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使用する場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、利用者にとって有益だと判断した順に項目を並べ替えていることを確認します。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを伝える指示ファイルを確認します。Web サイトへの機能追加方法に関するコンテキストを含む[ユーザーインターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則
   - Astro、Tailwind CSS、Drizzle などのファイル別指示へのリンク

> [!TIP]
> 指示ファイルでは、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きく複雑な内容を小さく分け、参考となる例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能の完成には、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルターロジックを追加または調整する
- フィルターの動作に関するテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを見つける様子を確認します。データレイヤーのヘルパー、UI、テストを特定し、ファイルの変更とテストの実行を始めます。

   ![Copilot がプロジェクトのファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、タスクの複雑さやコードベースによって数分かかる場合があります。作業中、コードエディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はありません。Copilot が完了すると、変更ごとに **Keep** または **Undo** が表示されます。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで作業します。

   ![Copilot Agent Mode のセッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次の内容を学習しました。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する仕組みを確認しました。
- GitHub Copilot Agent Mode でユーザーインターフェイス (UI) とデータレイヤーにまたがる新機能を実装しました。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューしました。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使って、Copilot にページネーションを実装させます。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- 利用者が1ページあたりの表示数を選べるようにするか、値を固定するか検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼し、コンテキストを提供しながら反復する場合があります。Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
