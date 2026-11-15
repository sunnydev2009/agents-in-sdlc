---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、Copilot cloud agent にユーザー体験の改善や機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつもあります。この mission control では、オープンな pull request (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次の内容を学習します。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を出し、機能を追加で依頼します。

## シナリオ

エージェントにハイコントラストモードを依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、同時に追加するのが合理的です。ハイコントラストに加えてライトモードも追加されるよう、エージェントを案内します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。次のようなタスクが表示されます。
   - コードのドキュメントを更新する
   - ゲーム変更用 API を生成する
   - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを1つ選び、Copilot が行った作業を確認します。次の内容が含まれる場合があります。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - Model Context Protocol (MCP) サーバーをセットアップする
   - 割り当てられたタスクを完了するための各種作業を行う

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。PR と変更されたファイルを確認して、作業を監視できます。

## cloud agent に指示を出す

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始したことが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中の cloud agent タスク。Steer active session while Copilot is working ダイアログが強調表示されています](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加したことを確認します。

## Copilot の作業を待つ

更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい要望が作業に組み込まれます。

これまでと同様に、完了まで数分かかります。その間に、これまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って、GitHub Copilot cloud agent のタスクを監視し、案内する方法を確認しました。この mission control では、進行中および完了したタスクを追跡し、実行中の作業をレビューし、途中のタスクの範囲を変更したり追加の指示を与えたりできます。

次の内容を確認しました。

- mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[作成された pull request を反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
