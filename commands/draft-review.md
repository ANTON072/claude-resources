---
name: draft-review
description: 指定されたPRのコードレビューを実施し、pending reviewとして投稿する
tools:
  - bash
---

# PR レビューと pending 投稿

`$ARGUMENTS` で指定された PR をレビューし、GitHub の pending review として下書き保存します。

---

## ステップ 1 — PR の内容を把握する

```bash
gh pr view $ARGUMENTS
gh pr diff $ARGUMENTS
```

差分と PR の説明を読み、変更の意図・影響範囲・懸念点を整理してください。

---

## ステップ 2 — レビューを実施する

`agents code-reviewer` を使ってコードレビューを行い、指摘事項をまとめてください。

---

## ステップ 3 — pending review として投稿する

### GitHub Reviews API の動作

`event` フィールドの有無がレビューの公開状態を決定します。

| `event` の指定      | 結果                              |
| ------------------- | --------------------------------- |
| **省略（推奨）**    | pending 保存（本コマンドの目的）  |
| `"COMMENT"`         | 即時公開                          |
| `"APPROVE"`         | 承認として即時公開                |
| `"REQUEST_CHANGES"` | 変更要求として即時公開            |

**`event` を JSON に含めてはいけません。** 一度 submit されたレビューを pending 状態に戻す手段は GitHub API に存在しないため、誤って公開した場合は個別コメントを削除するしかありません。投稿前に JSON 本文に `"event"` キーが存在しないことを必ず確認してください。

### コメントフィールドの仕様

| フィールド   | 説明                                                               |
| ------------ | ------------------------------------------------------------------ |
| `path`       | リポジトリルートからの相対パス                                     |
| `body`       | コメント本文                                                       |
| `line`       | 対象行のファイル内行番号（差分上の相対位置ではない）               |
| `side`       | 追加・変更行は `"RIGHT"`、削除行は `"LEFT"`                        |
| `start_line` | 複数行コメント時のみ指定（開始行番号）                             |
| `start_side` | `start_line` を指定する場合は必須（通常 `"RIGHT"`）                |

### 投稿コマンド

```bash
REPO=$(gh repo view --json nameWithOwner -q .nameWithOwner)
PR_NUMBER=$ARGUMENTS

# JSON に "event" キーが含まれていないことを確認してから実行すること
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

レスポンスの `state` が `"PENDING"` であることを確認してください。`"COMMENTED"` / `"APPROVED"` / `"CHANGES_REQUESTED"` の場合は即時公開されており、ユーザーに報告してください。

### 修正提案を含める場合

GitHub の suggestion 形式をコメント本文に埋め込めます。

```json
{
  "body": "全体サマリー",
  "comments": [
    {
      "path": "src/example.py",
      "line": 42,
      "side": "RIGHT",
      "body": "変数名をより意図が伝わるものにすることを提案します：\n\n```suggestion\nuser_count = len(users)\n```"
    }
  ]
}
```

---

## ステップ 4 — 完了報告

投稿完了後、以下を伝えてください。

- pending review として保存されたこと
- GitHub UI で内容を確認・編集してから submit が必要なこと
- 対象 PR の URL

---

## チェックリスト

- [ ] JSON に `event` キーが含まれていない
- [ ] `line` はファイル内の絶対行番号（差分上の相対位置ではない）
- [ ] 複数行コメントは `start_line` と `start_side` を両方指定している
- [ ] 投稿後のレスポンスで `"state": "PENDING"` を確認した
