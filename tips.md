# Tips

- [x] ジョブ間の依存関係を表現したい
- [ ] 実行コマンドを共通化したい
- [ ] 1リポジトリ内でアクションを作成し、ワークフローのジョブ内で利用したい
- [ ] ジョブ・アクション間で値を共有したい
- [ ] feature/main間のイベント制御したい

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

## 1リポジトリ内でアクションを作成し、ワークフローのジョブ内で利用したい

> アクション、ワークフロー、アプリケーションコードを 1 つのリポジトリで組み合わせる予定の場合、アクションは .github ディレクトリに保存することをお勧めします。 たとえば、.github/actions/action-aや.github/actions/action-bに保存します。

上にある通り、利用できる。

- [アクションについて - GitHub Docs](https://docs.github.com/ja/actions/creating-actions/about-actions#choosing-a-location-for-your-action)
