# Tips

- [x] ジョブ間の依存関係を表現したい
- [x] 実行コマンドを共通化したい
- [x] ローカルリポジトリ内でアクションを作成し、ワークフローのジョブ内で利用したい
- [x] ジョブ・アクション間で値を共有したい
- [x] ブランチ名でジョブ実行可否を制御したい
- [x] 環境変数を設定・参照したい
- [ ] ActionsのYAMLにLintをかけたい
- [x] [ローカルでActionsをテストしたい](./local.md)
- [x] タイムアウトをジョブ全体で同じ値を設定したい
- [ ] tagのpushをトリガーに *connventional commits* 形式の *Releases* を作りたい

## ジョブ間の依存関係を表現したい

*needs* で可能。以下の例はjob1,job2,job3の順で実行される。job3にはステータスチェック関数 *always()*を定義しているので、依存ジョブの実行結果に関わらずに実行される。

```yml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    if: always()
    needs: [job1, job2]
```

## 実行コマンドを共通化したい

複合実行ステップアクション（composite-run-steps-action）により、アクションをパッケージ化できる。このアクションはワークフローと同一リポジトリでも提供可能。

https://docs.github.com/ja/actions/creating-actions/creating-a-composite-run-steps-action

[GitHub Actionsのメタデータ構文 - GitHub Docs](https://docs.github.com/ja/actions/creating-actions/metadata-syntax-for-github-actions#runs-for-composite-run-steps-actions) より、 *uses* は使えないので、Actionsは使えない。

## ローカルリポジトリでアクションを作成し、ワークフローのジョブ内で利用したい

> アクション、ワークフロー、アプリケーションコードを 1 つのリポジトリで組み合わせる予定の場合、アクションは .github ディレクトリに保存することをお勧めします。 たとえば、.github/actions/action-aや.github/actions/action-bに保存します。

上にある通り、利用できる。

- [アクションについて - GitHub Docs](https://docs.github.com/ja/actions/creating-actions/about-actions#choosing-a-location-for-your-action)

ローカルにアクションを書いて使う場合、 *uses* のパスに注意。

```yml:workflow.yml
# workflow
jobs:
  job1:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - id: foo
        uses: ./.github/actions/greet # format {org}/{repo}[/path]
        with:
          input-id: 'value' 
```

## ブランチ名でジョブ実行可否を制御したい

組み込み関数の *contains*, *startsWith/endsWith* と、 githubコンテキストの *github.ref* で制御できる。

- [GitHub Actions のコンテキストおよび式の構文 - GitHub Docs](https://docs.github.com/ja/actions/reference/context-and-expression-syntax-for-github-actions#functions)

## 環境変数を設定・参照したい

1. ジョブ全体設定（ *env* ）
[GitHub Actionsのワークフロー構文 - GitHub Docs](https://docs.github.com/ja/actions/reference/workflow-syntax-for-github-actions#)

2. アクション間設定（ *echo "{name}={value}" >> $GITHUB_ENV* ）
[GitHub Actionsのワークフローコマンド - GitHub Docs](https://docs.github.com/ja/actions/reference/workflow-commands-for-github-actions#setting-an-environment-variable) ※ 環境変数作成・更新と参照は一緒のアクションではできない。

## タイムアウトをジョブ全体で同じ値を設定したい

job毎に設定するのは面倒なので、ジョブ全体で一律に設定したいが、サポートされていないらしい。

```yml
name: xxx
on: push
defaults:
  timeout-minutes: 0.5 # 30s
jobs:

# error message
# Unexpected value 'timeout-minutes'
```

### ref

- [Cannot set default.timeout-minutes - Code to Cloud / GitHub Actions - GitHub Support Community](https://github.community/t/cannot-set-default-timeout-minutes/118164)

## パスとブランチ名でイベントを制御したい

```yml
# ブランチ main へのPushイベントで、パス ./main に変更がある場合にジョブを実行する。
on:
  push:
    branches:
      - main
    paths:
      - 'main'
```
