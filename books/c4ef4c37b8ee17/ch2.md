---
title: "権限管理"
---

# ポリシーの作成

### 用意するもの

- リポジトリ ARN
    以下より確認できます。
    デベロッパー用ツール > CodeCommit > リポジトリ > [リポジトリ名] > 設定

### 制限したいアクション

- プッシュ
- ブランチの削除
- マネージドコンソールからファイルの追加
- ブランチのマージ
- プルリクのマージ

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Deny",
            "Action": [
                "codecommit:GitPush",
                "codecommit:DeleteBranch",
                "codecommit:PutFile",
                "codecommit:MergeBranches*",
                "codecommit:MergePullRequest*"
            ],
            "Resource": "[リポジトリARN]",
            "Condition": {
                "StringEqualsIfExists": {
                    "codecommit:References": [
                        "refs/heads/main",
                        "refs/heads/develop",
                        "refs/heads/hotfix",
                        "refs/heads/release"
                    ]
                },
                "Null": {
                    "codecommit:References": "false"
                }
            }
        }
    ]
}
```
