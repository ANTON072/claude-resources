---
name: post-pending-review
description: 指定されたPRのコードレビューを実施し、pending reviewとして投稿する
tools:
  - bash
---

# PRレビュー作業

PR $ARGUMENTS のコードレビューを実施してください

## 実行手順

### 1. PR情報の取得

ghコマンドでPRの詳細を取得してください：

```bash
gh pr view $ARGUMENTS
gh pr diff $ARGUMENTS
```

### 2. レビュー実施

agents code-reviewer を利用してコードレビューを実施してください

### 3. レビュー結果をpending reviewとして投稿

`gh api` を使って直接投稿してください。JSONファイルは作成せず、ヒアドキュメントで直接渡します。

#### ⚠️ 最重要: `event` フィールドを絶対に含めないこと

GitHub Reviews API の仕様上、pending review として保存するには **`event` フィールドを送信してはいけません**。

| `event` の値        | 結果                                                 |
| ------------------- | ---------------------------------------------------- |
| **未指定（省略）**  | ✅ **pending review として保存**（本コマンドの目的） |
| `"COMMENT"`         | ❌ 即時公開（全員に見える）                          |
| `"APPROVE"`         | ❌ 承認として即時公開                                |
| `"REQUEST_CHANGES"` | ❌ 変更要求として即時公開                            |

一度 submit されたレビューは GitHub API から pending 状態に戻せません（個別コメント削除のみ可）。pending 目的で event を付けると取り返しがつかないため、**JSON に event キーを書かないこと**。投稿前に生成する JSON 本文に "event" が含まれていないことを必ず確認してください。

投稿後のレスポンスに `"state":"PENDING"` が含まれていることを確認してください。`"COMMENTED"` / `"APPROVED"` / `"CHANGES_REQUESTED"` になっていたら失敗（公開されてしまった状態）です。

#### フィールド説明

- `path`: 対象ファイルのリポジトリルートからの相対パス
- `body`: コメント内容
- `line`: コメント対象の最終行（ファイル中の実際の行番号。差分上の相対位置ではない）
- `side`: 追加/変更行は `"RIGHT"`、削除行は `"LEFT"`
- `start_line`: 複数行コメントの場合のみ指定（開始行番号）
- `start_side`: `start_line` を指定する場合は必須（通常は `"RIGHT"`）
- `event`: **絶対に指定しない**（トップレベルのフィールド。上記参照）

#### 投稿コマンド

```bash
REPO=$(gh repo view --json nameWithOwner -q .nameWithOwner)
PR_NUMBER=$ARGUMENTS

# event フィールドを含んでいないことを必ず確認すること（含めると即時公開される）
gh api "repos/${REPO}/pulls/${PR_NUMBER}/reviews" \
  --method POST \
  --input - << 'EOF'
{
  "body": "レビュー全体のサマリー",
  "comments": [
    {
      "path": "対象ファイルのパス",
      "body": "コメント内容",
      "line": 42,
      "side": "RIGHT"
    }
  ]
}
EOF
```

投稿後、レスポンスの `state` フィールドを確認し、`PENDING` でなければユーザーに報告して対応を仰ぐこと。

#### 修正提案がある場合

`comments` 配列に含めるオブジェクトの `body` で、以下のようにGitHub suggestion形式を使用してください：

````json
{
  "body": "レビュー全体のサマリー",
  "comments": [
    {
      "path": "src/example.py",
      "line": 42,
      "side": "RIGHT",
      "body": "変数名をより明確にすることを提案します：\n\n```suggestion\nuser_count = len(users)\n```"
    }
  ]
}
````

### 4. 完了報告

投稿完了後、以下を報告してください：

- pending review が作成されたこと
- ユーザーが GitHub UI で確認・編集後に submit する必要があること
- PR の URL

## 注意事項

- **絶対に `event` フィールドを JSON に含めないこと**（含めると即時公開され、pending 状態に戻せない）
- 投稿後は必ずレスポンスの `state` が `PENDING` であることを確認する
- `line` はファイル中の実際の行番号を指定する（差分上の相対位置ではない）
- 複数行コメントの場合は `start_line` と `start_side` の両方を指定する
- コメントがない場合は `comments` を空配列にして全体コメントのみ投稿可能
- pending review は作成者本人にのみ見え、submit するまで他の人には見えない
