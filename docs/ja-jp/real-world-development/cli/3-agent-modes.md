---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチに合意し、Autopilot で Issue の内容からフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。しかし、より大きな変更には、さらに堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存のフローに沿って作業できるため、適切なものを適切な方法で構築できます。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスを実践します。Issue から新機能を作り、コードと機能を検証し、最後にプロジェクトへマージします。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや pull request (PR) を使いますが、ここでは中心的な概念に集中できるよう手順を短縮します。

このレッスンでは、次の内容を学習します。

- GitHub Issue から Copilot との会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

機能の開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリーとパブリッシャーでゲームを絞り込める機能が必要になりました。バックログの Issue には機能が記載されていますが、カテゴリーの組み合わせ方など、コーディング前に合意が必要です。Plan モードで決定事項を整理し、Autopilot による範囲を限定した実装を許可します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、基本はさらに重要になります。多くの開発者は、次のようなフローに従います。

1. 必要な作業を記載した Issue を開きます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 正確な手順はチームや Organization によって異なりますが、多くの場合は上記の流れを調整したものです。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、人が作成したコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントの自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: 開発者とエージェントが共同で作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成し、計画中はプロジェクトファイルを編集できません。
- **Autopilot**: エージェントは自律してコードを作成し、テストを実行して、タスクが完了するまで反復します。

Plan モードで開始して計画をレビューし、その後 Autopilot で実装します。

## Issue から作業を開始する

フィルター機能に着手する前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) から始まることを確認します。`#` を使うと、GitHub 上の Issue や pull request (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に作業を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、事前に計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 先ほど追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に沿って回答します。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決定した内容と合わない箇所があれば、Copilot に計画の修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。Autopilot では、Copilot が完了したと判断するまで問題に取り組み、反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンに表示される同等のオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があるため、先に計画をレビューしてください。依存関係の不足やポート競合が報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、他のコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないか検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容に照らし合わせます。
4. コードレビューが終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードに問題がないように見えても、実際に動くか確認しましょう。アプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. 次のプロンプトでアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリー、パブリッシャー、および計画で合意した組み合わせを試します。
4. リセットと結果なしの場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot との会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]、文書化されたプラクティスにコードを従わせる方法を詳しく確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
