# ローカルでActionsをテストする

[nektos/act: Run your GitHub Actions locally 🚀](https://github.com/nektos/act)を使う。

```sh
# ワークフロー一覧
$ act -l

# ジョブ実行（ 'needs' が含まれている場合依存先のジョブも実行する）
$ act -j <job_id/job_name>

# ジョブ実行（dry-run）
$ act -n -j <job_id/job_name>

# シークレットを含むジョブ実行
# https://github.com/nektos/act#secrets
$ act -j <job_name> -s MY_SECRET=SECRET_VALUE

# あるイベントトリガーのジョブ実行
$ act push
```

---

## composite-run-stepsを使うジョブが失敗する

[Support composite GitHub actions · Issue #339 · nektos/act](https://github.com/nektos/act/issues/339)で上がっている。
