# Git Auto-Commit

コミットメッセージを生成し、自動的にコミットを実行する。

## フラグ

- `--no-difit` : コミット後のdifit起動をスキップする

## General Rules

- コミットメッセージは常に日本語で書く

## Workflow

### 1. 全変更をステージング

    git add -A

`.env` や秘密鍵など意図しないファイルが含まれていないか `--stat` で確認してから次に進む。

### 2. ステージ済み変更の確認

    git diff --cached --stat

空の場合は処理を中断する。

### 3. 差分の分析

ステップ2の `--stat` 結果の変更行数（insertions + deletions の合計）で判断する:

- **300行未満** → `git diff --cached` で全差分を読む
- **300行以上** → stat の結果から変更規模の大きいファイルを選択し、`Read` で個別に参照する

必要に応じて:

- 新規ファイルはファイル全体を読む
- git log --oneline -5 でコンテキストを補う

### 4. コミットメッセージの生成と実行

Conventional Commits形式:

    <type>[(<scope>)]: <subject>

type: feat, fix, docs, style, refactor, perf, test, chore
scope: 任意

ルール:

- subject: 命令形、小文字始まり、末尾ピリオドなし、50字以内

実行:

    git commit -m "type(scope): subject"

コミットが失敗した場合（pre-commitフックなど）はエラー内容をユーザーに伝えて中断する。

### 5. レビュー画面の起動

`--no-difit` フラグが指定された場合はこのステップをスキップする。

コミット完了後、`difit` でレビュー画面を起動してユーザーに確認を促す。

    difit

ブラウザが開いたら「実装内容をご確認ください。」と伝える。

## 実行例

    git commit -m "fix(auth): トークンの有効期限チェックを修正"
