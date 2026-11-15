---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で進めたい場合、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も伝えられます。フィルター機能を作成したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインがあります。

- コメントでは、コードの繰り返しではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、TypeScript documentation (TSDoc) または JavaScript documentation (JSDoc) のコメントで、目的、パラメーター、戻り値を記述します。該当する場合は、注入可能な `db` 引数についても記述します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変わったらコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示では、Copilot にコンテキストや設定を伝え、コーディングスタイルや要件に合った提案やコードを生成しやすくします。好みの規約、ライブラリ、コードコメントの形式などを指定できます。リポジトリ全体、または特定のファイルやタスクを対象にできます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに対するガイドラインを記載します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

開始用プロジェクトには指示ファイルが用意されています。変更の影響を確認するため、まず既存の内容を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。ターミナルではなくエディターで開いてください。
3. プロジェクトの概要とコーディング指針を確認します。この指示は、リポジトリ内で Copilot を使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、Copilot が参照する例を示せます。

## チームの方針に合わせて指示ファイルを更新する

次に、`copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。35行目付近にあります。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を読み込ませるため、機能のコンテキストを保ったまま会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新した指示を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、機能に新しい標準を適用しました。具体的には、次の作業を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される仕組みを確認しました。
- コーディング標準が守られるように指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
