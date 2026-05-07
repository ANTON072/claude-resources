---
name: pr
description: "インテリジェントなベースブランチ検出でプルリクエストを作成する。使用タイミング：(1) ユーザーが「PRを作成」「PRを作る」「PRを開く」と言った場合、(2) ユーザーが現在のブランチからPRを作成したい場合、(3) ユーザーがフィーチャーブランチでの作業を完了してPRが必要な場合。会話、gitの履歴、またはリポジトリのデフォルトからベースブランチを自動検出する。"
argument-hint: "[target-branch]"
---

# PRコマンド

会話履歴、gitログ、またはリポジトリのデフォルトからの自動ベースブランチ検出でプルリクエストを作成します。

## 使い方

- `/pr` — 自動検出されたベースブランチでPRを作成する
- `/pr <target-branch>` — 指定されたブランチを対象とするPRを作成する

## 手順

### 1. 現在の状態を確認する

```bash
git status
git branch --show-current
```

フィーチャーブランチにいることを確認する（main/master/developではない）。

### 2. ベースブランチを決定する

**ベースブランチ検出の優先順位：**

1. **明示的な引数**：`<target-branch>` が提供された場合は直接使用する
2. **会話のコンテキスト**：以下の会話履歴を検索する：
   - "based on"、"branch from"、"create branch from"
   - 対象ブランチを言及するPRのコンテキストまたはIssue参照
   - ブランチ作成を示す最近のgitコマンド
3. **gitの履歴**：gitログとmerge-baseから自動検出する
4. **リポジトリのデフォルト**：フォールバックとしてデフォルトブランチを使用する

#### 会話履歴の確認

ベースブランチに関する手がかりを現在の会話から検索する：

- 「developから作成」「feature-xに基づいて」などのフレーズを探す
- ユーザーがどのブランチから開始したかを言及しているか確認する
- 対象を示すIssue/PRのコンテキストを探す

#### gitの履歴からの検出

```bash
# 最新のリモートrefsを取得
git fetch origin

# 候補ブランチとのmerge-baseを検索
for branch in main master develop staging; do
  git merge-base HEAD origin/$branch 2>/dev/null && echo "Found: $branch"
done

# どのブランチが最も近いmerge-baseを持つか確認
# 最近の共通の祖先を持つブランチが親の可能性が高い
```

#### リポジトリのデフォルトへのフォールバック

```bash
# デフォルトブランチを取得
gh repo view --json defaultBranchRef --jq '.defaultBranchRef.name'
```

### 3. 既存のPRを確認する

```bash
gh pr list --head $(git branch --show-current)
```

PRが存在する場合は、閲覧するか更新するかユーザーに確認する。

### 4. 変更を分析する

```bash
# コミットを取得
git log origin/<base-branch>..HEAD --oneline

# ファイルの変更を取得
git diff origin/<base-branch>..HEAD --stat
```

### 5. ユーザーに確認する

作成前に確認する：

- **現在のブランチ**：`<branch-name>`
- **対象ベースブランチ**：`<base-branch>`（検出方法：「指定」「会話から」「自動検出」「デフォルト」）
- **コミット**：含まれるコミット数
- **提案するPRタイトル**：コミット/ブランチ名に基づいて

### 6. プッシュしてPRを作成する

**オプション：Copilotによる本文の下書き**

本文を手動で下書きする前に、Copilotによる本文の下書きを試みる：

```bash
DRAFT=$($HOME/.claude/skills/gco/scripts/gco-pr-body.sh "<base-branch>" 2>/dev/null || true)
```

`$DRAFT` が空でない場合は、本文の出発点として使用する。Claudeは必ずそれを確認・調整する — 抜けを補完し、不正確な点を修正し、トーン/完全性を確保する。スクリプトが失敗または空を返した場合は、本文を直接下書きする。

```bash
# 必要に応じてプッシュ
git push -u origin $(git branch --show-current)

# PRを作成
gh pr create \
  --base <base-branch> \
  --title "<title>" \
  --body "$(cat <<'EOF'
## サマリー
<説明>

## 変更内容
- <変更1>
- <変更2>

## テストプラン
<テスト手順>
EOF
)"
```

Copilotの出力はそのまま適用しない — PRを作成する前に必ず確認・調整する。

### 7. 結果を報告する

以下を表示する：

- PR URL
- PR番号
- ベースブランチ（どのように決定されたか）
- ヘッドブランチ

## 検出ロジック

このコマンドはベースブランチを見つけるために以下のロジックを使用する：

1. **ユーザー指定**：最高優先度 — 引数を直接使用する
2. **会話分析**：ブランチのコンテキストを会話から確認する
3. **gitのmerge-base**：候補ブランチとの最も近い共通の祖先を見つける
4. **デフォルトブランチ**：`gh repo view` を使用した最終フォールバック

検出が曖昧な場合は、PRを作成する前にユーザーに明確化を求める。

## 重要な注意事項

- 確認なしにmain/masterを仮定しない
- ベースブランチがどのように決定されたかを常にユーザーに伝える
- PRを作成する前にブランチがプッシュされていることを確認する
- 意味のあるPRタイトルと説明を含める
