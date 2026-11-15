---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot が把握すべき背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは指示追加の前後で同じプロンプトを試しません。Copilot cloud agent は GitHub Issue 上で非同期に動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、cloud agent が作成した pull request をレビューするときに指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関する次のガイドラインがあります。

- データレイヤーには必ず単体テストを追加します。
- ユーザーインターフェイス (UI) はダークモードで、現代的な見た目にします。
- TypeScript documentation (TSDoc) のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示では、Copilot にコンテキストや設定を伝え、コーディングスタイルや要件に合ったコード提案を得られます。規約、ライブラリ、コードコメントの形式などを指定できます。リポジトリ全体、または特定の種類のファイルを対象にできます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語 (TypeScript や Astro など) や、UI コンポーネントまたは単体テストの作成などのタスクに対するガイドラインを記載します。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[カスタム指示の対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイル作成の全般的な説明はこのワークショップの範囲外ですが、サンプルプロジェクトの例は一般的な方法を示しています。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに対する指針を記載します。
- 自然な言葉を使い、明確な指針と、望ましいコードおよび望ましくないコードの例を示します。

指示ファイルの作成や AI の使い方に唯一の正解はありません。プロジェクトに合う方法は、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトのファイルには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の指示があります。
>
> Copilot に指示ファイルの生成を依頼することもできます。画面ごとに操作方法が異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要な場合は各ハーネスで説明します。
>
> テンプレートやひな形を探す場合は、指示ファイル、カスタムエージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイルを読みます。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの短い説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの相対 glob で、指示の対象となるファイルを指定します。ここでは `**/*.test.ts` に一致する TypeScript のテストファイルなどが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールし、`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、生成時に参照する例を示せます。ここに記載されたパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーコメントの規則を追加します。

## リポジトリ全体の標準を追加する

ここでは、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーをコードに追加する、プロジェクト共通の規則を追加します。後の演習で cloud agent に Issue を割り当てる際、この規則が適用されます。

編集の前にブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、変更を整理してコミットします。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を伝えるためのファイルです。コード提案の改善に役立つよう、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。27行目付近にあります。コーディング標準は記載されていますが、生成されたヘルパーにドキュメントコメントがなかったのは、コード内ドキュメントの規則がないためです。
3. 既存の標準の直後に、次の Markdown を追加して、ファイルヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは `.github/copilot-instructions.md` でリポジトリ全体に適用することも、`*.instructions.md` で言語、ファイル種別、タスクに限定することもできます。今回追加したドキュメントコメント規則など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象となるブランチから指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、指示を反映させるには `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成し、マージします。次の演習で Issue に割り当てた cloud agent は、これらの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成せずに `main` へコミットしてもかまいません。実際のプロジェクトでの手順に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成する pull request に、この標準が適用される様子を確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
